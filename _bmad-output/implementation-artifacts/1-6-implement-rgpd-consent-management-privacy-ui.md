# Story 1.6: Implement RGPD Consent Management & Privacy UI

Status: ready-for-dev

---

## Story

As a user,
I want to view and manage my RGPD privacy settings,
So that I can exercise my data privacy rights (access, rectification, deletion).

---

## Acceptance Criteria

### AC1: Privacy Dashboard
**Given** I am logged in as any user type
**When** I navigate to `/settings/privacy`
**Then** I see the RGPD privacy dashboard with sections:
- "Politique de confidentialite" (link to full policy)
- "Vos donnees personnelles" (summary of what data is collected)
- "Vos droits RGPD" with actions:
  - "Acceder a mes donnees" (download personal data as JSON)
  - "Rectifier mes donnees" (link to profile edit)
  - "Supprimer mon compte" (delete account with confirmation)

### AC2: Data Export
**And** when I click "Acceder a mes donnees":
- API endpoint `/api/rgpd/export` generates JSON export of all my personal data
- Export includes: user info, organization info (if admin), activity logs
- Download starts automatically

### AC3: Account Deletion
**And** when I click "Supprimer mon compte":
- Confirmation modal appears: "Etes-vous sur de vouloir supprimer votre compte? Cette action est irreversible."
- If I confirm, API endpoint `/api/rgpd/delete` is called
- My user record is anonymized (email, phone, name replaced with anonymized values)
- Anonymization completes within 24h (NFR-S4)
- I am logged out and redirected to `/login` with message: "Votre compte a ete supprime"

### AC4: Admin Deletion Restriction
**And** if I am an ADMIN and try to delete my account while having active drivers:
- Deletion is blocked with error: "Vous devez d'abord supprimer tous les comptes chauffeurs avant de supprimer votre compte entreprise"

### AC5: Registration Consent
**And** during registration (`/register`), RGPD consent checkbox is displayed with text:
"J'accepte la [Politique de confidentialite](#) et les [Conditions Generales d'Utilisation](#)"

### AC6: Consent Timestamp
**And** consent timestamp is stored in database when user registers

---

## Tasks / Subtasks

- [ ] **Task 1: Update User Model for RGPD** (AC: #6)
  - [ ] 1.1 Add `rgpdConsentAt` DateTime field to User model
  - [ ] 1.2 Add `anonymizedAt` DateTime? field for deletion tracking
  - [ ] 1.3 Run Prisma migration

- [ ] **Task 2: Create Privacy Settings Page** (AC: #1)
  - [ ] 2.1 Create `/app/(dashboard)/settings/privacy/page.tsx`
  - [ ] 2.2 Add "Politique de confidentialite" section with link
  - [ ] 2.3 Add "Vos donnees personnelles" summary section
  - [ ] 2.4 Add "Vos droits RGPD" section with action buttons

- [ ] **Task 3: Create Static Policy Pages** (AC: #1)
  - [ ] 3.1 Create `/app/(public)/privacy/page.tsx` - Privacy policy
  - [ ] 3.2 Create `/app/(public)/terms/page.tsx` - Terms of service
  - [ ] 3.3 Add French legal content (placeholder)

- [ ] **Task 4: Implement Data Export API** (AC: #2)
  - [ ] 4.1 Create `/app/api/rgpd/export/route.ts`
  - [ ] 4.2 Gather user data (profile, preferences)
  - [ ] 4.3 Gather organization data (if admin)
  - [ ] 4.4 Gather activity logs
  - [ ] 4.5 Format as JSON with readable structure
  - [ ] 4.6 Set content-disposition for download
  - [ ] 4.7 Log export action for audit

- [ ] **Task 5: Implement Account Deletion API** (AC: #3, #4)
  - [ ] 5.1 Create `/app/api/rgpd/delete/route.ts`
  - [ ] 5.2 Check if user is ADMIN with active drivers
  - [ ] 5.3 Block deletion if drivers exist
  - [ ] 5.4 Anonymize user data (replace with placeholder values)
  - [ ] 5.5 Set anonymizedAt timestamp
  - [ ] 5.6 Destroy session and cookies
  - [ ] 5.7 Log deletion action for audit

- [ ] **Task 6: Create Deletion Confirmation Modal** (AC: #3)
  - [ ] 6.1 Create confirmation dialog component
  - [ ] 6.2 Add warning text about irreversibility
  - [ ] 6.3 Require explicit confirmation
  - [ ] 6.4 Handle deletion and redirect

- [ ] **Task 7: Update Registration Form** (AC: #5, #6)
  - [ ] 7.1 Update registration form with consent checkbox
  - [ ] 7.2 Add links to privacy policy and terms
  - [ ] 7.3 Store rgpdConsentAt on successful registration
  - [ ] 7.4 Ensure consent is required for registration

- [ ] **Task 8: Testing** (Validation)
  - [ ] 8.1 Test data export downloads correctly
  - [ ] 8.2 Test account deletion anonymizes data
  - [ ] 8.3 Test admin cannot delete with active drivers
  - [ ] 8.4 Test consent is required at registration
  - [ ] 8.5 Test consent timestamp is stored

---

## Dev Notes

### Dependencies on Previous Stories

- **Story 1.1-1.5**: Authentication system complete
- **Story 1.2**: User model with organizationId

### Prisma Schema Updates

```prisma
model User {
  // ... existing fields
  rgpdConsentAt DateTime?  // When user accepted RGPD
  anonymizedAt  DateTime?  // When user was anonymized (for deletion)
}
```

### Data Export API

```typescript
// app/api/rgpd/export/route.ts
import { NextResponse } from 'next/server';
import { getSession } from '@/lib/auth';
import { prisma } from '@/lib/db';

export async function GET() {
  const session = await getSession();
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
  }

  const user = await prisma.user.findUnique({
    where: { id: session.user.id },
    include: {
      organization: session.user.role === 'ADMIN',
    },
  });

  const exportData = {
    exportDate: new Date().toISOString(),
    userData: {
      email: user.email,
      name: user.name,
      firstName: user.firstName,
      phone: user.phone,
      role: user.role,
      createdAt: user.createdAt,
      rgpdConsentAt: user.rgpdConsentAt,
    },
    organizationData: user.organization ? {
      name: user.organization.name,
      createdAt: user.organization.createdAt,
    } : null,
  };

  // Log export action
  await prisma.auditLog.create({
    data: {
      userId: user.id,
      action: 'RGPD_EXPORT',
      organizationId: user.organizationId,
    },
  });

  return new NextResponse(JSON.stringify(exportData, null, 2), {
    headers: {
      'Content-Type': 'application/json',
      'Content-Disposition': `attachment; filename="flottebox-data-export-${Date.now()}.json"`,
    },
  });
}
```

### Account Deletion API

```typescript
// app/api/rgpd/delete/route.ts
import { NextResponse } from 'next/server';
import { getSession, signOut } from '@/lib/auth';
import { prisma } from '@/lib/db';

export async function DELETE() {
  const session = await getSession();
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
  }

  const user = await prisma.user.findUnique({
    where: { id: session.user.id },
  });

  // Check if admin with active drivers
  if (user.role === 'ADMIN') {
    const activeDrivers = await prisma.user.count({
      where: {
        organizationId: user.organizationId,
        role: 'DRIVER',
        anonymizedAt: null,
      },
    });

    if (activeDrivers > 0) {
      return NextResponse.json({
        error: 'Vous devez d\'abord supprimer tous les comptes chauffeurs avant de supprimer votre compte entreprise',
      }, { status: 400 });
    }
  }

  // Anonymize user data
  await prisma.user.update({
    where: { id: user.id },
    data: {
      email: `deleted-${user.id}@anonymized.local`,
      username: null,
      name: 'Utilisateur supprime',
      firstName: null,
      phone: null,
      passwordHash: null,
      anonymizedAt: new Date(),
    },
  });

  // Log deletion
  await prisma.auditLog.create({
    data: {
      userId: user.id,
      action: 'RGPD_DELETE',
      organizationId: user.organizationId,
    },
  });

  // Sign out user
  await signOut();

  return NextResponse.json({ success: true });
}
```

### Privacy Settings Page

```typescript
// app/(dashboard)/settings/privacy/page.tsx
'use client';

import { useState } from 'react';
import { Button } from '@/components/ui/button';
import { Card } from '@/components/ui/card';
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogTrigger,
} from '@/components/ui/alert-dialog';

export default function PrivacySettingsPage() {
  const [isDeleting, setIsDeleting] = useState(false);

  const handleExport = async () => {
    window.location.href = '/api/rgpd/export';
  };

  const handleDelete = async () => {
    setIsDeleting(true);
    try {
      const res = await fetch('/api/rgpd/delete', { method: 'DELETE' });
      if (res.ok) {
        window.location.href = '/login?message=account_deleted';
      } else {
        const data = await res.json();
        alert(data.error);
      }
    } finally {
      setIsDeleting(false);
    }
  };

  return (
    <div className="space-y-6">
      <h1>Parametres de confidentialite</h1>

      <Card>
        <h2>Politique de confidentialite</h2>
        <a href="/privacy">Consulter notre politique de confidentialite</a>
      </Card>

      <Card>
        <h2>Vos donnees personnelles</h2>
        <p>FlotteBox collecte les donnees suivantes:</p>
        <ul>
          <li>Email et informations de compte</li>
          <li>Donnees de l'entreprise et des vehicules</li>
          <li>Documents scannes</li>
          <li>Journaux d'activite</li>
        </ul>
      </Card>

      <Card>
        <h2>Vos droits RGPD</h2>

        <Button onClick={handleExport}>
          Acceder a mes donnees
        </Button>

        <Button variant="outline" asChild>
          <a href="/settings/profile">Rectifier mes donnees</a>
        </Button>

        <AlertDialog>
          <AlertDialogTrigger asChild>
            <Button variant="destructive">
              Supprimer mon compte
            </Button>
          </AlertDialogTrigger>
          <AlertDialogContent>
            <AlertDialogHeader>
              <AlertDialogTitle>Supprimer votre compte?</AlertDialogTitle>
              <AlertDialogDescription>
                Etes-vous sur de vouloir supprimer votre compte?
                Cette action est irreversible.
              </AlertDialogDescription>
            </AlertDialogHeader>
            <AlertDialogFooter>
              <AlertDialogCancel>Annuler</AlertDialogCancel>
              <AlertDialogAction onClick={handleDelete} disabled={isDeleting}>
                Supprimer definitivement
              </AlertDialogAction>
            </AlertDialogFooter>
          </AlertDialogContent>
        </AlertDialog>
      </Card>
    </div>
  );
}
```

### Anonymization Strategy

Per RGPD requirements, we anonymize rather than hard delete:

| Field | Original | Anonymized |
|-------|----------|------------|
| email | john@company.com | deleted-{userId}@anonymized.local |
| username | johndoe | null |
| name | John Doe | Utilisateur supprime |
| firstName | John | null |
| phone | +33612345678 | null |
| passwordHash | $2b$12$... | null |

### Consent Checkbox (Registration)

```typescript
// In registration form
<div className="flex items-center space-x-2">
  <Checkbox id="rgpdConsent" {...form.register('rgpdConsent')} />
  <label htmlFor="rgpdConsent">
    J'accepte la{' '}
    <a href="/privacy" target="_blank">Politique de confidentialite</a>
    {' '}et les{' '}
    <a href="/terms" target="_blank">Conditions Generales d'Utilisation</a>
  </label>
</div>
```

---

### Project Structure Notes

- **Fichiers a creer:**
  - `app/(dashboard)/settings/privacy/page.tsx`
  - `app/(public)/privacy/page.tsx`
  - `app/(public)/terms/page.tsx`
  - `app/api/rgpd/export/route.ts`
  - `app/api/rgpd/delete/route.ts`

- **Fichiers a modifier:**
  - `prisma/schema.prisma` - Add RGPD fields
  - `app/(auth)/register/page.tsx` - Add consent checkbox

---

### References

- [Source: prd.md#RGPD] - Data privacy requirements
- [Source: architecture.md#RGPD compliance] - Anonymization, 10 year retention
- [Source: epics.md#Story 1.6] - Acceptance criteria originaux
- [Source: NFR-S4] - 24h anonymization deadline

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
