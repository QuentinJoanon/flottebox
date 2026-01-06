## 5. Spécifications techniques

### 5.1 Stack technique

**Frontend Web & Mobile (PWA) :**
```yaml
Framework: Next.js 16 (App Router + React 19)
Language: TypeScript 5.x
Styling: Tailwind CSS 4.x + shadcn/ui
State Management: ...
Forms: React Hook Form + Zod (validation)
PDF Viewer: react-pdf
Charts: Recharts
Icons: Lucide React
```

**Backend (BaaS) :**
```yaml
Database: Supabase (PostgreSQL)
Storage: Supabase Storage (S3-compatible)
Auth: Supabase Auth
Realtime: Supabase Realtime (si besoin)
Edge Functions: Supabase Edge Functions (Deno)
```

**Services tiers :**
```yaml
Paiements: LemonSqueezy (avec gestion automatique de la TVA)
Emails: Resend (emails transactionnels + alertes)
OCR: Mistral OCR 3 (mistral-ocr-2512) - $2/1000 pages via API standard
Monitoring: Sentry (erreurs) + Vercel Analytics
Analytics: Plausible ou Posthog (privacy-first)
Support: Widget feedback in-app (custom) + bouton d'aide dans sidebar
```

**Infrastructure :**
```yaml
Hosting Frontend: Vercel
Hosting Backend: Supabase Cloud
CI/CD: GitHub Actions + Vercel auto-deploy
Domain: OVH ou Cloudflare
CDN: Vercel Edge Network
```

### 5.2 Architecture logicielle

```
┌─────────────────────────────────────────────────────┐
│                  CLIENT (Browser/PWA)               │
│                                                     │
│  Next.js 16 App (React 19)                         │
│  ├── /app/dashboard  (gestionnaires)               │
│  ├── /app/mobile     (vue mobile simplifiée)       │
│  └── /app/api        (API routes - proxy)          │
│                                                     │
│  UI: shadcn/ui + Tailwind CSS                      │
└──────────────────┬──────────────────────────────────┘
                   │
                   │ HTTPS (Supabase Client SDK)
                   │
┌──────────────────▼──────────────────────────────────┐
│              SUPABASE (Backend as a Service)        │
│                                                     │
│  ┌─────────────────────────────────────────┐      │
│  │  PostgreSQL Database                    │      │
│  │  - companies                            │      │
│  │  - company_overrides (quotas custom)    │      │
│  │  - users                                │      │
│  │  - vehicles                             │      │
│  │  - documents                            │      │
│  │  - alerts                               │      │
│  │  - activity_logs                        │      │
│  │                                         │      │
│  │  Row Level Security (RLS) activé       │      │
│  └─────────────────────────────────────────┘      │
│                                                     │
│  ┌─────────────────────────────────────────┐      │
│  │  Storage (S3-compatible)                │      │
│  │  Buckets:                               │      │
│  │  - documents/ (documents utilisateurs)  │      │
│  │  - avatars/ (photos profil)             │      │
│  │  - vehicles/ (photos véhicules)         │      │
│  └─────────────────────────────────────────┘      │
│                                                     │
│  ┌─────────────────────────────────────────┐      │
│  │  Auth (JWT + Magic Links)               │      │
│  │  - Email/Password                       │      │
│  │  - Email verification                   │      │
│  │  - Password reset                       │      │
│  └─────────────────────────────────────────┘      │
│                                                     │
│  ┌─────────────────────────────────────────┐      │
│  │  Edge Functions (Deno)                  │      │
│  │  - generate-daily-alerts                │      │
│  │  - send-alert-emails                    │      │
│  │  - cleanup-expired-sessions             │      │
│  └─────────────────────────────────────────┘      │
└─────────────────────────────────────────────────────┘
                   │
                   │ Webhooks / CRON
                   │
┌──────────────────▼──────────────────────────────────┐
│              SERVICES EXTERNES                      │
│                                                     │
│  LemonSqueezy (paiements + TVA)                    │
│  Resend (emails transactionnels)                   │
│  Sentry (monitoring erreurs)                       │
└─────────────────────────────────────────────────────┘
```

### 5.3 Schéma de base de données

```sql
-- Extensions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Companies (tenants)
CREATE TABLE companies (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  siret TEXT UNIQUE,
  email TEXT,
  phone TEXT,
  logo_url TEXT,
  subscription_plan TEXT DEFAULT 'starter' CHECK (subscription_plan IN ('starter', 'pro', 'business', 'team', 'enterprise')),
  subscription_status TEXT DEFAULT 'trial' CHECK (subscription_status IN ('trial', 'active', 'canceled', 'past_due', 'expired')),
  trial_end_date TIMESTAMP WITH TIME ZONE, -- Date de fin de l'essai gratuit (NULL si souscription payante)
  lemonsqueezy_customer_id TEXT,
  lemonsqueezy_subscription_id TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Company Overrides (quotas et add-ons personnalisés par le super admin)
CREATE TABLE company_overrides (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE UNIQUE,

  -- Quotas custom (NULL = utiliser quota du plan)
  custom_vehicle_limit INTEGER, -- Override limite véhicules
  custom_user_limit INTEGER, -- Override limite utilisateurs
  custom_ocr_monthly_quota INTEGER, -- Quota OCR mensuel gratuit (NULL = pay-as-you-go standard)

  -- Add-ons offerts gratuitement
  free_api_access BOOLEAN DEFAULT FALSE, -- API access offert
  free_phone_support BOOLEAN DEFAULT FALSE, -- Support téléphone offert
  free_advanced_export BOOLEAN DEFAULT FALSE, -- Export comptable avancé offert

  -- Tarif custom
  custom_monthly_price DECIMAL(10, 2), -- Prix mensuel négocié (NULL = prix du plan standard)

  -- Métadonnées
  notes TEXT, -- Notes internes (pourquoi ces overrides ?)
  created_by UUID REFERENCES users(id), -- Super admin qui a créé l'override
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Index pour performance
CREATE INDEX idx_company_overrides_company ON company_overrides(company_id);

-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY REFERENCES auth.users ON DELETE CASCADE,
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  role TEXT DEFAULT 'user' CHECK (role IN ('admin', 'manager', 'driver')),
  is_super_admin BOOLEAN DEFAULT FALSE, -- Quentin uniquement
  first_name TEXT,
  last_name TEXT,
  phone TEXT,
  avatar_url TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Vehicles
CREATE TABLE vehicles (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  type TEXT NOT NULL CHECK (type IN ('truck', 'van', 'trailer', 'construction')),
  license_plate TEXT NOT NULL,
  brand TEXT,
  model TEXT,
  year INTEGER,
  vin TEXT,
  mileage INTEGER,
  purchase_date DATE,
  photo_url TEXT,
  assigned_driver_id UUID REFERENCES users(id) ON DELETE SET NULL,
  notes TEXT,
  is_archived BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  UNIQUE(company_id, license_plate)
);

-- Document types enum
CREATE TYPE document_type AS ENUM (
  'carte_grise',
  'assurance',
  'controle_technique',
  'permis_conduire',
  'fimo_fco',
  'visite_medicale',
  'contrat_location',
  'facture_entretien',
  'autre'
);

-- Documents
CREATE TABLE documents (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  vehicle_id UUID REFERENCES vehicles(id) ON DELETE CASCADE,
  type document_type NOT NULL,
  file_path TEXT NOT NULL,
  file_size INTEGER,
  file_mime_type TEXT,
  issue_date DATE,
  expiry_date DATE,
  document_number TEXT,
  notes TEXT,
  uploaded_by UUID REFERENCES users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Alerts
CREATE TABLE alerts (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  vehicle_id UUID REFERENCES vehicles(id) ON DELETE CASCADE,
  document_id UUID REFERENCES documents(id) ON DELETE CASCADE,
  alert_type TEXT CHECK (alert_type IN ('info', 'warning', 'urgent', 'critical')),
  alert_date DATE NOT NULL,
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'sent', 'dismissed', 'snoozed')),
  snoozed_until DATE,
  message TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Activity logs
CREATE TABLE activity_logs (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE SET NULL,
  action TEXT NOT NULL,
  entity_type TEXT,
  entity_id UUID,
  metadata JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Events tracking (for super admin analytics)
CREATE TABLE events (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE SET NULL,
  event_name TEXT NOT NULL,
  event_data JSONB, -- Metadata specific to event
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- OCR Usage tracking (pour facturation et quotas)
CREATE TABLE ocr_usage (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE SET NULL,
  document_type TEXT, -- Type de document scanné (carte_grise, ct, assurance, etc.)
  document_count INTEGER DEFAULT 1, -- Nombre de documents traités (pour imports multiples)
  cost DECIMAL(10, 2), -- Coût total (0.10€ × document_count)
  was_free BOOLEAN DEFAULT FALSE, -- true si couvert par quota gratuit
  batch_id UUID, -- Si import multiple, même batch_id pour tous
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Index pour calcul du quota mensuel
CREATE INDEX idx_ocr_usage_company_date ON ocr_usage(company_id, created_at);
CREATE INDEX idx_ocr_usage_batch ON ocr_usage(batch_id);

-- Feedback from clients
CREATE TABLE feedback (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE SET NULL,
  type TEXT CHECK (type IN ('bug', 'feature_request', 'question', 'other')),
  message TEXT NOT NULL,
  screenshot_url TEXT,
  status TEXT DEFAULT 'new' CHECK (status IN ('new', 'in_progress', 'resolved', 'closed')),
  admin_response TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Admin notes on companies (private)
CREATE TABLE company_notes (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id) ON DELETE CASCADE,
  note TEXT NOT NULL,
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_vehicles_company ON vehicles(company_id);
CREATE INDEX idx_vehicles_driver ON vehicles(assigned_driver_id);
CREATE INDEX idx_documents_vehicle ON documents(vehicle_id);
CREATE INDEX idx_documents_expiry ON documents(expiry_date);
CREATE INDEX idx_alerts_vehicle ON alerts(vehicle_id);
CREATE INDEX idx_alerts_status ON alerts(status);
CREATE INDEX idx_alerts_date ON alerts(alert_date);
CREATE INDEX idx_activity_logs_company ON activity_logs(company_id);
CREATE INDEX idx_events_company ON events(company_id);
CREATE INDEX idx_events_name ON events(event_name);
CREATE INDEX idx_events_created ON events(created_at);
CREATE INDEX idx_feedback_company ON feedback(company_id);
CREATE INDEX idx_feedback_status ON feedback(status);

-- Row Level Security (RLS)
ALTER TABLE companies ENABLE ROW LEVEL SECURITY;
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE vehicles ENABLE ROW LEVEL SECURITY;
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;
ALTER TABLE alerts ENABLE ROW LEVEL SECURITY;
ALTER TABLE activity_logs ENABLE ROW LEVEL SECURITY;

-- RLS Policies

-- Companies: Users can only see their own company
CREATE POLICY "Users can view own company"
  ON companies FOR SELECT
  USING (id = (SELECT company_id FROM users WHERE id = auth.uid()));

-- Users: Users can see users from their company
CREATE POLICY "Users can view company users"
  ON users FOR SELECT
  USING (company_id = (SELECT company_id FROM users WHERE id = auth.uid()));

-- Vehicles: Users can see vehicles from their company
CREATE POLICY "Users can view company vehicles"
  ON vehicles FOR SELECT
  USING (company_id = (SELECT company_id FROM users WHERE id = auth.uid()));

-- Drivers can only see their assigned vehicles
CREATE POLICY "Drivers can view assigned vehicles"
  ON vehicles FOR SELECT
  USING (
    assigned_driver_id = auth.uid() OR
    (SELECT role FROM users WHERE id = auth.uid()) IN ('admin', 'manager')
  );

-- Documents: Users can see documents from their company's vehicles
CREATE POLICY "Users can view company documents"
  ON documents FOR SELECT
  USING (
    vehicle_id IN (
      SELECT id FROM vehicles WHERE company_id = (
        SELECT company_id FROM users WHERE id = auth.uid()
      )
    )
  );

-- Similar policies for INSERT, UPDATE, DELETE based on roles

-- Functions

-- Function to calculate compliance status
CREATE OR REPLACE FUNCTION get_vehicle_compliance_status(vehicle_uuid UUID)
RETURNS TEXT AS $$
DECLARE
  expired_count INTEGER;
  expiring_soon_count INTEGER;
BEGIN
  -- Count expired documents
  SELECT COUNT(*) INTO expired_count
  FROM documents
  WHERE vehicle_id = vehicle_uuid
    AND expiry_date IS NOT NULL
    AND expiry_date < CURRENT_DATE;
  
  -- Count documents expiring in next 30 days
  SELECT COUNT(*) INTO expiring_soon_count
  FROM documents
  WHERE vehicle_id = vehicle_uuid
    AND expiry_date IS NOT NULL
    AND expiry_date BETWEEN CURRENT_DATE AND (CURRENT_DATE + INTERVAL '30 days');
  
  IF expired_count > 0 THEN
    RETURN 'critical';
  ELSIF expiring_soon_count > 0 THEN
    RETURN 'warning';
  ELSE
    RETURN 'ok';
  END IF;
END;
$$ LANGUAGE plpgsql;

-- Trigger to update updated_at timestamp
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_companies_updated_at BEFORE UPDATE ON companies
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_vehicles_updated_at BEFORE UPDATE ON vehicles
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_documents_updated_at BEFORE UPDATE ON documents
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_alerts_updated_at BEFORE UPDATE ON alerts
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

### 5.4 Structure du projet Next.js 16

```
flottebox/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   ├── register/
│   │   └── reset-password/
│   ├── (dashboard)/
│   │   ├── layout.tsx           # Layout avec sidebar
│   │   ├── page.tsx             # Dashboard homepage
│   │   ├── vehicles/
│   │   │   ├── page.tsx         # Liste véhicules
│   │   │   ├── [id]/
│   │   │   │   └── page.tsx     # Détail véhicule
│   │   │   └── new/
│   │   │       └── page.tsx     # Ajouter véhicule
│   │   ├── documents/
│   │   │   ├── page.tsx         # Liste tous docs
│   │   │   └── [id]/
│   │   │       └── page.tsx     # Détail document
│   │   ├── alerts/
│   │   │   └── page.tsx         # Dashboard alertes
│   │   └── settings/
│   │       ├── page.tsx         # Paramètres compte
│   │       ├── users/
│   │       └── billing/
│   ├── (mobile)/
│   │   ├── layout.tsx           # Layout mobile (bottom nav)
│   │   ├── vehicles/
│   │   ├── scan/
│   │   │   └── page.tsx         # Scanner document
│   │   ├── alerts/
│   │   └── profile/
│   ├── (superadmin)/
│   │   ├── layout.tsx           # Layout super admin
│   │   ├── page.tsx             # Dashboard analytics global
│   │   ├── clients/
│   │   │   ├── page.tsx         # Liste clients avec filtres
│   │   │   └── [id]/
│   │   │       └── page.tsx     # Détail client 360°
│   │   ├── analytics/
│   │   │   ├── page.tsx         # Métriques détaillées
│   │   │   └── events/
│   │   │       └── page.tsx     # Event tracking
│   │   ├── feedback/
│   │   │   └── page.tsx         # Feedbacks clients
│   │   ├── demo/
│   │   │   └── page.tsx         # Gestion mode démo
│   │   └── segments/
│   │       └── page.tsx         # Segments & campagnes
│   ├── api/
│   │   ├── vehicles/
│   │   │   ├── route.ts         # GET, POST vehicles
│   │   │   └── [id]/
│   │   │       └── route.ts     # GET, PATCH, DELETE vehicle
│   │   ├── documents/
│   │   │   ├── upload/
│   │   │   │   └── route.ts     # POST upload document
│   │   │   └── [id]/
│   │   │       └── route.ts     # GET, DELETE document
│   │   ├── alerts/
│   │   │   └── route.ts
│   │   ├── admin/
│   │   │   ├── clients/
│   │   │   │   └── route.ts     # GET all clients (super admin)
│   │   │   ├── analytics/
│   │   │   │   └── route.ts     # GET metrics
│   │   │   ├── events/
│   │   │   │   └── route.ts     # Track events
│   │   │   └── impersonate/
│   │   │       └── route.ts     # POST impersonate client
│   │   └── webhooks/
│   │       ├── lemonsqueezy/
│   │       └── supabase/
│   ├── layout.tsx               # Root layout
│   ├── page.tsx                 # Landing page
│   └── globals.css
│
├── components/
│   ├── ui/                      # shadcn/ui components
│   ├── dashboard/
│   │   ├── stats-cards.tsx
│   │   ├── vehicle-card.tsx
│   │   ├── alert-badge.tsx
│   │   └── recent-activity.tsx
│   ├── vehicles/
│   │   ├── vehicle-form.tsx
│   │   ├── vehicle-list.tsx
│   │   └── vehicle-filters.tsx
│   ├── documents/
│   │   ├── document-upload.tsx
│   │   ├── document-preview.tsx
│   │   └── camera-capture.tsx   # PWA camera
│   └── layout/
│       ├── header.tsx
│       ├── sidebar.tsx
│       └── mobile-nav.tsx
│
├── lib/
│   ├── supabase/
│   │   ├── client.ts            # Client-side Supabase
│   │   ├── server.ts            # Server-side Supabase
│   │   └── middleware.ts
│   ├── api/
│   │   ├── vehicles.ts
│   │   ├── documents.ts
│   │   └── alerts.ts
│   ├── quotas/
│   │   ├── check-quotas.ts         # Vérification quotas avec overrides
│   │   └── ocr-usage.ts            # Gestion usage OCR
│   ├── utils/
│   │   ├── date-helpers.ts
│   │   ├── file-helpers.ts
│   │   └── validation.ts
│   └── hooks/
│       ├── use-vehicles.ts
│       ├── use-documents.ts
│       └── use-auth.ts
│
├── types/
│   ├── database.ts              # Generated from Supabase
│   ├── vehicle.ts
│   ├── document.ts
│   └── alert.ts
│
├── public/
│   ├── icons/                   # PWA icons
│   ├── manifest.json
│   └── sw.js                    # Service Worker
│
├── supabase/
│   ├── migrations/
│   └── seed.sql
│
├── .env.local
├── .env.example
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

### 5.5 Configuration PWA

```javascript
// next.config.js
const withPWA = require('@ducanh2912/next-pwa').default({
  dest: 'public',
  register: true,
  skipWaiting: true,
  disable: process.env.NODE_ENV === 'development',
})

module.exports = withPWA({
  // Next.js config
})

// public/manifest.json
{
  "name": "FlotteBox - Gestion de flotte",
  "short_name": "FlotteBox",
  "description": "Gérez vos documents de flotte en toute simplicité",
  "start_url": "/mobile",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "orientation": "portrait",
  "icons": [
    {
      "src": "/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "screenshots": [
    {
      "src": "/screenshots/desktop.png",
      "sizes": "1280x720",
      "type": "image/png",
      "form_factor": "wide"
    },
    {
      "src": "/screenshots/mobile.png",
      "sizes": "750x1334",
      "type": "image/png",
      "form_factor": "narrow"
    }
  ]
}
```

### 5.6 Gestion des quotas et overrides (exemples de code)

**Fichier : `lib/quotas/check-quotas.ts`**

```typescript
import { createClient } from '@/lib/supabase/server'

// Plans et leurs limites par défaut
const PLAN_LIMITS = {
  starter: { vehicles: 30, users: 1, ocr_monthly: null }, // null = pay-as-you-go
  pro: { vehicles: 30, users: 3, ocr_monthly: null },
  business: { vehicles: 50, users: 5, ocr_monthly: null },
  team: { vehicles: 100, users: 10, ocr_monthly: null },
  enterprise: { vehicles: 200, users: 50, ocr_monthly: 50 }, // 50 OCR/mois offerts
}

export async function getCompanyQuotas(companyId: string) {
  const supabase = createClient()

  // Récupérer le plan de l'entreprise
  const { data: company } = await supabase
    .from('companies')
    .select('subscription_plan')
    .eq('id', companyId)
    .single()

  const planLimits = PLAN_LIMITS[company.subscription_plan]

  // Récupérer les overrides s'ils existent
  const { data: override } = await supabase
    .from('company_overrides')
    .select('*')
    .eq('company_id', companyId)
    .single()

  // Appliquer les overrides sur les limites du plan
  return {
    vehicleLimit: override?.custom_vehicle_limit ?? planLimits.vehicles,
    userLimit: override?.custom_user_limit ?? planLimits.users,
    ocrMonthlyQuota: override?.custom_ocr_monthly_quota ?? planLimits.ocr_monthly,
    freeApiAccess: override?.free_api_access ?? false,
    freePhoneSupport: override?.free_phone_support ?? false,
    customPrice: override?.custom_monthly_price ?? null,
    hasOverrides: !!override,
  }
}

export async function canAddVehicle(companyId: string): Promise<{ allowed: boolean; reason?: string }> {
  const supabase = createClient()
  const quotas = await getCompanyQuotas(companyId)

  // Compter les véhicules actuels
  const { count } = await supabase
    .from('vehicles')
    .select('*', { count: 'exact', head: true })
    .eq('company_id', companyId)
    .eq('is_archived', false)

  if (count >= quotas.vehicleLimit) {
    return {
      allowed: false,
      reason: `Limite de ${quotas.vehicleLimit} véhicules atteinte. Passez au plan supérieur.`
    }
  }

  return { allowed: true }
}

export async function canAddUser(companyId: string): Promise<{ allowed: boolean; reason?: string }> {
  const supabase = createClient()
  const quotas = await getCompanyQuotas(companyId)

  const { count } = await supabase
    .from('users')
    .select('*', { count: 'exact', head: true })
    .eq('company_id', companyId)

  if (count >= quotas.userLimit) {
    return {
      allowed: false,
      reason: `Limite de ${quotas.userLimit} utilisateurs atteinte.`
    }
  }

  return { allowed: true }
}
```

**Fichier : `lib/quotas/ocr-usage.ts`**

```typescript
import { createClient } from '@/lib/supabase/server'

export async function trackOcrUsage(params: {
  companyId: string
  userId: string
  documentType: string
  documentCount: number
  batchId?: string
}) {
  const supabase = createClient()
  const quotas = await getCompanyQuotas(params.companyId)

  // Calculer l'usage OCR du mois en cours
  const startOfMonth = new Date()
  startOfMonth.setDate(1)
  startOfMonth.setHours(0, 0, 0, 0)

  const { data: monthlyUsage } = await supabase
    .from('ocr_usage')
    .select('document_count')
    .eq('company_id', params.companyId)
    .gte('created_at', startOfMonth.toISOString())
    .eq('was_free', true)

  const usedFreeQuota = monthlyUsage?.reduce((sum, row) => sum + row.document_count, 0) ?? 0

  // Déterminer combien de documents sont gratuits
  let freeCount = 0
  let paidCount = params.documentCount

  if (quotas.ocrMonthlyQuota !== null) {
    const remainingQuota = quotas.ocrMonthlyQuota - usedFreeQuota
    freeCount = Math.min(params.documentCount, remainingQuota)
    paidCount = params.documentCount - freeCount
  }

  // Enregistrer l'usage gratuit
  if (freeCount > 0) {
    await supabase.from('ocr_usage').insert({
      company_id: params.companyId,
      user_id: params.userId,
      document_type: params.documentType,
      document_count: freeCount,
      cost: 0,
      was_free: true,
      batch_id: params.batchId,
    })
  }

  // Enregistrer l'usage payant
  if (paidCount > 0) {
    const cost = paidCount * 0.10
    await supabase.from('ocr_usage').insert({
      company_id: params.companyId,
      user_id: params.userId,
      document_type: params.documentType,
      document_count: paidCount,
      cost: cost,
      was_free: false,
      batch_id: params.batchId,
    })
  }

  return {
    freeCount,
    paidCount,
    totalCost: paidCount * 0.10,
    remainingQuota: quotas.ocrMonthlyQuota ? quotas.ocrMonthlyQuota - usedFreeQuota - freeCount : null
  }
}

export async function getMonthlyOcrCost(companyId: string, year: number, month: number): Promise<number> {
  const supabase = createClient()

  const startDate = new Date(year, month - 1, 1)
  const endDate = new Date(year, month, 1)

  const { data } = await supabase
    .from('ocr_usage')
    .select('cost')
    .eq('company_id', companyId)
    .eq('was_free', false)
    .gte('created_at', startDate.toISOString())
    .lt('created_at', endDate.toISOString())

  return data?.reduce((sum, row) => sum + parseFloat(row.cost), 0) ?? 0
}
```

**Usage dans une API route :**

```typescript
// app/api/vehicles/route.ts
import { canAddVehicle } from '@/lib/quotas/check-quotas'

export async function POST(req: Request) {
  const session = await getSession()
  const companyId = session.user.company_id

  // Vérifier le quota avant d'ajouter
  const check = await canAddVehicle(companyId)

  if (!check.allowed) {
    return NextResponse.json(
      { error: check.reason },
      { status: 403 }
    )
  }

  // Créer le véhicule...
  const vehicle = await createVehicle(...)

  return NextResponse.json({ vehicle })
}
```

---

