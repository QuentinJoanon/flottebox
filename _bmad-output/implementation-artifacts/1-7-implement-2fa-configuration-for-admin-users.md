# Story 1.7: Implement 2FA Configuration for Admin Users

Status: ready-for-dev

---

## Story

As an admin user,
I want to configure Two-Factor Authentication (2FA) using TOTP,
So that my account has an additional layer of security.

---

## Acceptance Criteria

### AC1: 2FA Settings Section
**Given** I am logged in as an ADMIN user
**When** I navigate to `/settings/security`
**Then** I see a "Configurer l'authentification a deux facteurs (2FA)" section

### AC2: Enable 2FA - QR Code
**And** when I click "Activer 2FA":
- A QR code is generated using TOTP standard (RFC 6238)
- QR code is displayed with instructions: "Scannez ce code avec Google Authenticator ou Authy"
- A secret key is shown as text backup: "Cle secrete: ABCD-EFGH-IJKL-MNOP"

### AC3: Verification Code Input
**And** I am prompted to enter a 6-digit verification code from my authenticator app

### AC4: Successful 2FA Activation
**And** when I submit a valid code:
- 2FA is enabled for my account
- Database field `twoFactorEnabled` is set to true
- Recovery codes (10 single-use codes) are generated and displayed
- I see success message: "2FA active avec succes. Conservez vos codes de recuperation en lieu sur."

### AC5: Invalid Code Handling
**And** when I submit an invalid code:
- I see error: "Code invalide. Veuillez reessayer."
- I can attempt verification again

### AC6: 2FA Login Requirement
**And** after 2FA is enabled, subsequent logins require:
- Email + password validation first
- Then 6-digit TOTP code input
- Only after valid TOTP code is session created

### AC7: Disable 2FA
**And** I can disable 2FA by:
- Navigating to `/settings/security`
- Clicking "Desactiver 2FA"
- Entering current TOTP code to confirm
- 2FA is disabled and `twoFactorEnabled` is set to false

### AC8: Recovery Codes
**And** if I lose access to my authenticator, I can use one of my recovery codes:
- Each recovery code can only be used once
- After use, the code is marked as consumed in database

---

## Tasks / Subtasks

- [ ] **Task 1: Update User Model for 2FA** (AC: #4)
  - [ ] 1.1 Add `twoFactorEnabled` Boolean field (default false)
  - [ ] 1.2 Add `twoFactorSecret` String? field (encrypted)
  - [ ] 1.3 Run Prisma migration

- [ ] **Task 2: Create RecoveryCode Model** (AC: #4, #8)
  - [ ] 2.1 Create RecoveryCode model in Prisma
  - [ ] 2.2 Fields: id, userId, code, usedAt
  - [ ] 2.3 Run migration

- [ ] **Task 3: Create Security Settings Page** (AC: #1)
  - [ ] 3.1 Create `/app/(dashboard)/settings/security/page.tsx`
  - [ ] 3.2 Add 2FA section
  - [ ] 3.3 Show current 2FA status
  - [ ] 3.4 Add Enable/Disable button based on status

- [ ] **Task 4: Implement 2FA Setup Flow** (AC: #2, #3, #4)
  - [ ] 4.1 Install otplib package for TOTP
  - [ ] 4.2 Create `/app/api/2fa/setup/route.ts`
  - [ ] 4.3 Generate TOTP secret
  - [ ] 4.4 Generate QR code URL
  - [ ] 4.5 Return QR code and secret to frontend
  - [ ] 4.6 Display QR code with qrcode.react

- [ ] **Task 5: Implement 2FA Verification** (AC: #3, #4, #5)
  - [ ] 5.1 Create `/app/api/2fa/verify/route.ts`
  - [ ] 5.2 Validate TOTP code against secret
  - [ ] 5.3 If valid: enable 2FA, generate recovery codes
  - [ ] 5.4 If invalid: return error
  - [ ] 5.5 Return recovery codes to display

- [ ] **Task 6: Generate Recovery Codes** (AC: #4, #8)
  - [ ] 6.1 Create recovery code generation function
  - [ ] 6.2 Generate 10 unique codes
  - [ ] 6.3 Hash codes before storing (can only verify, not retrieve)
  - [ ] 6.4 Display codes to user (only time they'll see them)

- [ ] **Task 7: Update Login Flow for 2FA** (AC: #6)
  - [ ] 7.1 After password validation, check if 2FA enabled
  - [ ] 7.2 If 2FA: redirect to `/login/2fa` page
  - [ ] 7.3 Create TOTP input page
  - [ ] 7.4 Validate TOTP code
  - [ ] 7.5 Create session only after valid 2FA

- [ ] **Task 8: Implement Recovery Code Login** (AC: #8)
  - [ ] 8.1 Add "Utiliser un code de recuperation" link
  - [ ] 8.2 Create recovery code input form
  - [ ] 8.3 Validate recovery code against hashed codes
  - [ ] 8.4 Mark code as used after successful auth

- [ ] **Task 9: Implement 2FA Disable** (AC: #7)
  - [ ] 9.1 Create `/app/api/2fa/disable/route.ts`
  - [ ] 9.2 Require current TOTP code to disable
  - [ ] 9.3 Set twoFactorEnabled to false
  - [ ] 9.4 Delete stored secret and recovery codes

- [ ] **Task 10: Testing** (Validation)
  - [ ] 10.1 Test 2FA setup flow with authenticator app
  - [ ] 10.2 Test login with 2FA enabled
  - [ ] 10.3 Test invalid TOTP code rejection
  - [ ] 10.4 Test recovery code login
  - [ ] 10.5 Test recovery code single-use
  - [ ] 10.6 Test 2FA disable flow

---

## Dev Notes

### Dependencies on Previous Stories

- **Story 1.1-1.5**: Authentication system complete
- **Story 1.5**: Login flow exists

### Required Packages

```bash
pnpm add otplib qrcode.react
pnpm add -D @types/qrcode.react
```

### Prisma Schema Updates

```prisma
model User {
  // ... existing fields
  twoFactorEnabled Boolean   @default(false)
  twoFactorSecret  String?   // Encrypted TOTP secret
  recoveryCodes    RecoveryCode[]
}

model RecoveryCode {
  id        String    @id @default(cuid())
  userId    String
  user      User      @relation(fields: [userId], references: [id])
  codeHash  String    // Hashed recovery code
  usedAt    DateTime?
  createdAt DateTime  @default(now())

  @@index([userId])
}
```

### TOTP Setup API

```typescript
// app/api/2fa/setup/route.ts
import { authenticator } from 'otplib';
import { NextResponse } from 'next/server';
import { getSession } from '@/lib/auth';
import { prisma } from '@/lib/db';
import { encrypt } from '@/lib/encryption';

export async function POST() {
  const session = await getSession();
  if (!session || session.user.role !== 'ADMIN') {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
  }

  // Generate secret
  const secret = authenticator.generateSecret();

  // Generate otpauth URL for QR code
  const otpauthUrl = authenticator.keyuri(
    session.user.email,
    'FlotteBox',
    secret
  );

  // Temporarily store secret (not enabled yet until verified)
  await prisma.user.update({
    where: { id: session.user.id },
    data: { twoFactorSecret: encrypt(secret) },
  });

  return NextResponse.json({
    secret: formatSecret(secret), // ABCD-EFGH-IJKL-MNOP format
    otpauthUrl,
  });
}

function formatSecret(secret: string): string {
  return secret.match(/.{1,4}/g)?.join('-') || secret;
}
```

### TOTP Verification API

```typescript
// app/api/2fa/verify/route.ts
import { authenticator } from 'otplib';
import { NextResponse } from 'next/server';
import { getSession } from '@/lib/auth';
import { prisma } from '@/lib/db';
import { decrypt } from '@/lib/encryption';
import { generateRecoveryCodes } from '@/lib/2fa';

export async function POST(req: Request) {
  const { code } = await req.json();
  const session = await getSession();

  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
  }

  const user = await prisma.user.findUnique({
    where: { id: session.user.id },
  });

  if (!user?.twoFactorSecret) {
    return NextResponse.json({ error: 'Setup required' }, { status: 400 });
  }

  const secret = decrypt(user.twoFactorSecret);
  const isValid = authenticator.verify({ token: code, secret });

  if (!isValid) {
    return NextResponse.json({
      error: 'Code invalide. Veuillez reessayer.',
    }, { status: 400 });
  }

  // Enable 2FA and generate recovery codes
  const recoveryCodes = await generateRecoveryCodes(user.id);

  await prisma.user.update({
    where: { id: user.id },
    data: { twoFactorEnabled: true },
  });

  return NextResponse.json({
    success: true,
    recoveryCodes, // Display once to user
    message: '2FA active avec succes. Conservez vos codes de recuperation en lieu sur.',
  });
}
```

### Recovery Codes Generation

```typescript
// lib/2fa.ts
import { randomBytes, createHash } from 'crypto';
import { prisma } from '@/lib/db';

export async function generateRecoveryCodes(userId: string): Promise<string[]> {
  // Delete existing recovery codes
  await prisma.recoveryCode.deleteMany({ where: { userId } });

  const codes: string[] = [];

  for (let i = 0; i < 10; i++) {
    // Generate 8-character alphanumeric code
    const code = randomBytes(4).toString('hex').toUpperCase();
    codes.push(code);

    // Store hashed version
    const codeHash = createHash('sha256').update(code).digest('hex');
    await prisma.recoveryCode.create({
      data: { userId, codeHash },
    });
  }

  return codes;
}

export async function verifyRecoveryCode(userId: string, code: string): Promise<boolean> {
  const codeHash = createHash('sha256').update(code.toUpperCase()).digest('hex');

  const recoveryCode = await prisma.recoveryCode.findFirst({
    where: {
      userId,
      codeHash,
      usedAt: null,
    },
  });

  if (!recoveryCode) return false;

  // Mark as used
  await prisma.recoveryCode.update({
    where: { id: recoveryCode.id },
    data: { usedAt: new Date() },
  });

  return true;
}
```

### 2FA Login Flow

```typescript
// Modified login flow
async function handleLogin(email: string, password: string) {
  // 1. Validate credentials
  const user = await validateCredentials(email, password);
  if (!user) throw new Error('Invalid credentials');

  // 2. Check if 2FA enabled
  if (user.twoFactorEnabled) {
    // Create temporary token for 2FA verification
    const tempToken = await createTempToken(user.id);
    return { requiresTwoFactor: true, tempToken };
  }

  // 3. Create session if no 2FA
  return { session: await createSession(user.id) };
}

// 2FA verification step
async function verifyTwoFactor(tempToken: string, code: string) {
  const userId = await verifyTempToken(tempToken);
  const user = await prisma.user.findUnique({ where: { id: userId } });

  const secret = decrypt(user.twoFactorSecret);
  const isValid = authenticator.verify({ token: code, secret });

  if (!isValid) {
    throw new Error('Code invalide. Veuillez reessayer.');
  }

  return { session: await createSession(userId) };
}
```

### Security Settings Page

```typescript
// app/(dashboard)/settings/security/page.tsx
'use client';

import { useState } from 'react';
import { QRCodeSVG } from 'qrcode.react';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';

export default function SecuritySettingsPage() {
  const [setupData, setSetupData] = useState<{
    secret: string;
    otpauthUrl: string;
  } | null>(null);
  const [verificationCode, setVerificationCode] = useState('');
  const [recoveryCodes, setRecoveryCodes] = useState<string[]>([]);

  const startSetup = async () => {
    const res = await fetch('/api/2fa/setup', { method: 'POST' });
    const data = await res.json();
    setSetupData(data);
  };

  const verifyCode = async () => {
    const res = await fetch('/api/2fa/verify', {
      method: 'POST',
      body: JSON.stringify({ code: verificationCode }),
    });
    const data = await res.json();

    if (data.success) {
      setRecoveryCodes(data.recoveryCodes);
      setSetupData(null);
    }
  };

  return (
    <div className="space-y-6">
      <h1>Securite du compte</h1>

      <section>
        <h2>Authentification a deux facteurs (2FA)</h2>

        {!setupData && recoveryCodes.length === 0 && (
          <Button onClick={startSetup}>Activer 2FA</Button>
        )}

        {setupData && (
          <div className="space-y-4">
            <p>Scannez ce code avec Google Authenticator ou Authy</p>
            <QRCodeSVG value={setupData.otpauthUrl} size={200} />
            <p>Cle secrete: {setupData.secret}</p>

            <div>
              <Input
                placeholder="Code a 6 chiffres"
                value={verificationCode}
                onChange={(e) => setVerificationCode(e.target.value)}
                maxLength={6}
              />
              <Button onClick={verifyCode}>Verifier</Button>
            </div>
          </div>
        )}

        {recoveryCodes.length > 0 && (
          <div className="space-y-4">
            <p className="text-green-600">
              2FA active avec succes. Conservez vos codes de recuperation en lieu sur.
            </p>
            <div className="grid grid-cols-2 gap-2 font-mono">
              {recoveryCodes.map((code, i) => (
                <div key={i} className="p-2 bg-gray-100 rounded">{code}</div>
              ))}
            </div>
          </div>
        )}
      </section>
    </div>
  );
}
```

### Security Considerations

- **Secret encryption**: TOTP secret encrypted at rest
- **Recovery codes hashed**: Cannot be retrieved, only verified
- **Single-use recovery codes**: Marked as used after consumption
- **Admin only**: 2FA available only for ADMIN role (highest privilege)
- **Require TOTP to disable**: Prevents unauthorized disabling

---

### Project Structure Notes

- **Fichiers a creer:**
  - `app/(dashboard)/settings/security/page.tsx`
  - `app/api/2fa/setup/route.ts`
  - `app/api/2fa/verify/route.ts`
  - `app/api/2fa/disable/route.ts`
  - `app/(auth)/login/2fa/page.tsx`
  - `lib/2fa.ts`
  - `lib/encryption.ts`

- **Fichiers a modifier:**
  - `prisma/schema.prisma` - Add 2FA fields
  - `lib/auth.ts` - 2FA in login flow

---

### References

- [Source: architecture.md#Authentication] - Better Auth with 2FA
- [Source: prd.md#Authentification] - 2FA for admin users (FR8)
- [Source: epics.md#Story 1.7] - Acceptance criteria originaux

---

## Dev Agent Record

### Agent Model Used

{{agent_model_name_version}}

### Debug Log References

_A remplir pendant l'implementation_

### Completion Notes List

_A remplir pendant l'implementation_

### File List

_A remplir pendant l'implementation avec la liste des fichiers crees/modifies_
