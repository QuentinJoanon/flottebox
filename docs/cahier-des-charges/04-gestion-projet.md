## 8. Planning de développement

---

### Phase 1 : Setup & Infrastructure + Tests (Semaine 1)

**Durée : 6 jours**

**Tâches :**
- [ ] Setup projet Next.js 16 + TypeScript
- [ ] Configuration Tailwind CSS + shadcn/ui
- [ ] Setup Supabase (projet, database, storage)
- [ ] Configuration variables d'environnement
- [ ] Génération types TypeScript depuis Supabase
- [ ] Setup Vercel + déploiement preview
- [ ] Configuration ESLint + Prettier
- [ ] Setup Sentry (monitoring)

**Livrable :** Projet initialisé, déployé sur Vercel preview

---

### Phase 2 : Auth & Multi-tenancy + LemonSqueezy (Semaine 2)

**Durée : 6-7 jours**

**Tâches :**
- [ ] Schéma DB : companies (avec trial_end_date, lemonsqueezy_subscription_id), users, company_overrides
- [ ] **Supabase Auth :**
  - [ ] Inscription classique (email + mot de passe)
  - [ ] **OAuth Google** (configuration dans Supabase Dashboard + boutons UI)
  - [ ] Login classique et Google
  - [ ] Reset password
- [ ] **Intégration LemonSqueezy :**
  - [ ] Compte LemonSqueezy créé (mode test)
  - [ ] Produits et prix configurés dans LemonSqueezy Dashboard
  - [ ] Installation `@lemonsqueezy/lemonsqueezy.js` (SDK)
  - [ ] API route : `/api/checkout/create-subscription` (création subscription avec trial)
  - [ ] Page `/register` avec LemonSqueezy Checkout intégré
  - [ ] Webhook LemonSqueezy : `order_created` (confirmation paiement)
  - [ ] Webhook LemonSqueezy : `subscription_updated` (fin de trial)
  - [ ] Webhook LemonSqueezy : `subscription_payment_failed` (paiement échoué)
- [ ] RLS policies (isolation par entreprise)
- [ ] Pages : /login, /register (avec LemonSqueezy), /reset-password
- [ ] Middleware protection routes (vérification statut subscription)
- [ ] Layout dashboard avec sidebar
- [ ] Badge "Essai gratuit - X jours" dans sidebar
- [ ] Gestion session (JWT refresh)

**Livrable :** Auth fonctionnelle (email + Google OAuth) + LemonSqueezy intégré + essai gratuit 14 jours opérationnel

---

### Phase 3 : CRUD Véhicules + Quotas + Import CSV (Semaine 3)

**Durée : 6 jours**

**Tâches :**
- [ ] Schéma DB : vehicles
- [ ] **Logique quotas :**
  - [ ] Création `lib/quotas/check-quotas.ts`
  - [ ] Création `lib/quotas/ocr-usage.ts`
  - [ ] Implémenter les fonctions de vérification des quotas
- [ ] API routes : GET, POST, PATCH, DELETE vehicles (avec vérification quota)
- [ ] Page : liste véhicules (tableau + filtres)
- [ ] Page : détail véhicule
- [ ] Page : formulaire ajout/édition véhicule
- [ ] Composant : vehicle-card
- [ ] Upload photo véhicule (Supabase Storage)
- [ ] **Import CSV véhicules + conducteurs**
  - [ ] Parser CSV (papaparse ou csv-parse)
  - [ ] Validation des données (Zod schemas)
  - [ ] Preview avant import
  - [ ] Rapport d'erreurs
  - [ ] Template CSV téléchargeable
  - [ ] Création/assignation automatique des conducteurs

**Livrable :** CRUD véhicules + système de quotas + import CSV fonctionnel

---

### Phase 4 : Gestion Documents (Semaine 4)

**Durée : 5-6 jours**

**Tâches :**
- [ ] Schéma DB : documents
- [ ] API routes : upload, delete, get documents
- [ ] Composant : document-upload (drag & drop)
- [ ] Intégration Supabase Storage (buckets par company)
- [ ] Preview PDF inline (react-pdf)
- [ ] Preview images
- [ ] Formulaire métadonnées document (type, dates)
- [ ] Liste documents par véhicule

**Livrable :** Upload et gestion documents fonctionnels

---

### Phase 5 : Alertes & Notifications (Semaine 5)

**Durée : 6 jours**

**Tâches :**
- [ ] Schéma DB : alerts
- [ ] Fonction calcul alertes (date d'expiration)
- [ ] Edge Function : génération alertes quotidiennes
- [ ] **Edge Function : rappels essai gratuit**
  - [ ] Vérification quotidienne des `trial_end_date`
  - [ ] Envoi emails J-7, J-3, J-1 (rappels avant débit automatique LemonSqueezy)
  - [ ] Note : le débit à J-14 et la conversion sont gérés automatiquement par LemonSqueezy (webhooks)
  - [ ] Gestion des comptes annulés : suppression données après 30 jours
- [ ] Intégration Resend (emails)
- [ ] Template email alertes
- [ ] **Templates emails essai gratuit** (J-7, J-3, J-0, dernière chance)
- [ ] Dashboard alertes (page dédiée)
- [ ] Widget alertes sur homepage
- [ ] **Badge essai gratuit dans sidebar** (affichage jours restants)
- [ ] Actions : marquer traité, snooze

**Livrable :** Système d'alertes automatiques opérationnel

---

### Phase 6 : PWA & Mobile (Semaine 6)

**Durée : 5 jours**

**Tâches :**
- [ ] Configuration PWA (manifest.json, service worker)
- [ ] Layout mobile (/mobile)
- [ ] Bottom navigation bar
- [ ] Page scan caméra
- [ ] Intégration getUserMedia API (caméra)
- [ ] Compression images avant upload
- [ ] Mode offline basique (cache lecture seule)
- [ ] Tests iOS Safari + Android Chrome

**Livrable :** PWA installable avec scan caméra fonctionnel

---

### Phase 7 : Dashboard & Stats (Semaine 7 - post-MVP)

**Durée : 3 jours**

**Tâches :**
- [ ] Widgets statistiques (Recharts)
- [ ] Graphique évolution alertes
- [ ] Timeline échéances 30 jours
- [ ] Activité récente
- [ ] Optimisation requêtes (agrégations)

**Livrable :** Dashboard complet avec visualisations

---

### Phase 8 : Polish & Déploiement (Semaine 7-8 - post-MVP)

**Durée : 2-3 jours**

**Tâches :**
- [ ] Tests utilisateurs (client ancre)
- [ ] Corrections bugs
- [ ] Optimisation performance (Lighthouse)
- [ ] SEO (metadata, sitemap)
- [ ] Documentation utilisateur basique
- [ ] Déploiement production

**Livrable :** MVP prêt pour premiers clients payants

---

### Phase 9 : Super Admin & Analytics (Semaines 9-10 - post-MVP prioritaire)

**Durée : 7-10 jours**

**Objectif :** Donner à Quentin les outils pour piloter son SaaS et identifier les opportunités de croissance.

**Tâches :**
- [ ] Table `events` + tracking automatique
- [ ] Dashboard analytics global (MRR, churn, ARPU, etc.)
- [ ] Liste clients avec filtres avancés
- [ ] Page détail client (vue 360°)
- [ ] Health score algorithmique
- [ ] Mode démo (3 scénarios pré-configurés)
- [ ] Table `feedback` + widget in-app
- [ ] Segments clients pré-définis
- [ ] Action "Impersonate" (se connecter en tant que client)
- [ ] Protection route super admin (middleware)

**Exemple de code - Middleware protection super admin :**
```typescript
// middleware.ts
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(req: NextRequest) {
  const res = NextResponse.next()
  const supabase = createMiddlewareClient({ req, res })
  
  // Check if route is super admin
  if (req.nextUrl.pathname.startsWith('/superadmin')) {
    const { data: { user } } = await supabase.auth.getUser()
    
    if (!user) {
      return NextResponse.redirect(new URL('/login', req.url))
    }
    
    // Check if user is super admin
    const { data: userData } = await supabase
      .from('users')
      .select('is_super_admin')
      .eq('id', user.id)
      .single()
    
    if (!userData?.is_super_admin) {
      return NextResponse.redirect(new URL('/dashboard', req.url))
    }
  }
  
  return res
}

export const config = {
  matcher: ['/superadmin/:path*']
}
```

**Exemple de code - Calcul MRR :**
```typescript
// app/api/admin/analytics/route.ts
export async function GET() {
  const supabase = await createClient()
  
  // MRR calculation
  const { data: companies } = await supabase
    .from('companies')
    .select('subscription_plan, created_at')
    .eq('subscription_status', 'active')
  
  const planPrices = {
    starter: 49,
    pro: 99,
    business: 199,
    team: 349
  }
  
  const mrr = companies?.reduce((sum, company) => {
    return sum + (planPrices[company.subscription_plan] || 0)
  }, 0)
  
  // Churn rate (last 30 days)
  const thirtyDaysAgo = new Date()
  thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30)
  
  const { count: churnedCount } = await supabase
    .from('companies')
    .select('*', { count: 'exact', head: true })
    .eq('subscription_status', 'canceled')
    .gte('updated_at', thirtyDaysAgo.toISOString())
  
  const { count: totalCount } = await supabase
    .from('companies')
    .select('*', { count: 'exact', head: true })
  
  const churnRate = (churnedCount / totalCount) * 100
  
  return Response.json({
    mrr,
    arr: mrr * 12,
    churnRate,
    totalClients: totalCount
  })
}
```

**Livrable :** Backoffice opérationnel pour piloter le business

---

### 📊 Récapitulatif du planning

**Durée totale MVP : 6-7 semaines**

| Phase | Durée |
|-------|-------|
| Phase 1 : Setup + Infrastructure | **6 jours** |
| Phase 2 : Auth + LemonSqueezy | **6-7 jours** |
| Phase 3 : Véhicules + Quotas | **6 jours** |
| Phase 4 : Documents | **5-6 jours** |
| Phase 5 : Alertes | **6 jours** |
| Phase 6 : PWA | **5 jours** |
| Phase 7-8 : Polish | **5 jours** |
| **TOTAL** | **39-41 jours** |

---

## 9. Tests & Qualité

### 9.1 Tests de compatibilité

**Navigateurs desktop :**
- Chrome (dernière version)
- Firefox (dernière version)
- Safari (macOS)
- Edge (dernière version)

**Navigateurs mobile :**
- iOS Safari (iOS 15+)
- Android Chrome (Android 10+)
- Samsung Internet (si disponible)

### 9.2 Performance

**Objectifs Lighthouse :**
- Performance : > 90
- Accessibility : > 90
- Best Practices : > 90
- SEO : > 90
- PWA : score parfait (installable, responsive, offline)

### 9.3 Sécurité et Conformité RGPD

#### 9.3.1 Protection des données sensibles

**Architecture sécurisée :**
- [ ] Chiffrement des données au repos (Supabase encryption at rest)
- [ ] Chiffrement des communications (HTTPS/TLS 1.3 obligatoire)
- [ ] Isolation stricte multi-tenant (RLS PostgreSQL)
- [ ] Pas de données sensibles en logs ou analytics
- [ ] Backup chiffré quotidien avec rétention 30 jours

**Contrôle d'accès :**
- [ ] Authentification forte (passwords hashed avec bcrypt)
- [ ] Session expiration (24h inactivité)
- [ ] Rate limiting sur endpoints sensibles (10 requêtes/minute)
- [ ] Gestion granulaire des permissions (admin, manager, driver)
- [ ] 2FA en option (post-MVP)

**Traçabilité :**
- [ ] Logs complets des accès aux documents (qui, quand, quelle action)
- [ ] Historique des modifications inaltérable
- [ ] Audit trail pour conformité légale
- [ ] Détection tentatives d'accès non autorisés

**Protection contre exfiltration :**
- [ ] Impossible de télécharger en masse (rate limit)
- [ ] Watermarking des PDF téléchargés avec identité utilisateur (post-MVP)
- [ ] Alertes en cas de téléchargements suspects (>20 docs/jour)
- [ ] Révocation instantanée des accès (départ salarié)

#### 9.3.2 Conformité RGPD

**Principes appliqués :**
- [ ] Minimisation des données (collecte strictement nécessaire)
- [ ] Finalité déterminée (gestion administrative flotte uniquement)
- [ ] Durée de conservation limitée (archivage après 5 ans)
- [ ] Sécurité et confidentialité by design

**Droits des personnes :**
- [ ] Droit d'accès : export de toutes les données personnelles
- [ ] Droit de rectification : modification via interface
- [ ] Droit à l'effacement : suppression compte + cascade
- [ ] Droit à la portabilité : export JSON/CSV
- [ ] Droit d'opposition : opt-out emails marketing

**Documentation obligatoire :**
- [ ] Registre des traitements (Article 30 RGPD)
- [ ] Mentions légales et politique de confidentialité
- [ ] Conditions générales d'utilisation
- [ ] DPA (Data Processing Agreement) pour les clients
- [ ] Procédure notification violations (72h max)

**Mesures organisationnelles :**
- [ ] Hébergement données EU (Supabase EU region)
- [ ] Sous-traitants RGPD-compliant (LemonSqueezy, Resend)
- [ ] Clauses contractuelles types
- [ ] Procédure réponse aux demandes (15 jours max)

#### 9.3.3 Checklist sécurité technique

- [ ] RLS activé sur toutes les tables sensibles
- [ ] Validation inputs côté serveur (Zod schemas)
- [ ] Protection CSRF (Next.js built-in + SameSite cookies)
- [ ] Sanitization uploads (vérification MIME types réels, pas juste extension)
- [ ] Limite taille fichiers (10 MB PDF, 5 MB images)
- [ ] Scan antivirus uploads (ClamAV ou VirusTotal API - post-MVP)
- [ ] Content Security Policy (CSP) headers
- [ ] X-Frame-Options, X-Content-Type-Options headers
- [ ] Secrets en variables d'environnement (jamais en clair dans le code)
- [ ] Dépendances à jour (Dependabot alerts)
- [ ] Pentesting avant production (ou audit externe)

---

## 10. Livrables

### 10.1 Livrables techniques

1. **Code source** :
   - Repository GitHub privé
   - Documentation README (setup, env vars)
   - Scripts de déploiement

2. **Base de données** :
   - Schéma SQL (migrations Supabase)
   - Script seed (données de test)

3. **Application déployée** :
   - URL production : https://flottebox.fr
   - URL staging : https://staging.flottebox.fr

4. **Documentation** :
   - Guide de démarrage développeur
   - Architecture technique (ce document)
   - API documentation (si exposée)

### 10.2 Livrables utilisateurs

1. **Application web/mobile** :
   - Dashboard gestionnaire
   - Vue mobile (PWA)
   - Scan caméra fonctionnel

2. **Documentation utilisateur** :
   - Guide de démarrage (vidéo 3 min)
   - FAQ
   - Tutoriel scan document

3. **Support** :
   - Email support@flottebox.fr
   - Chat Crisp (si budget)

---

## 11. Budget & Ressources

### 11.1 Coûts mensuels estimés

**Phase MVP (0-10 clients) :**
- Supabase : 0€ (tier gratuit)
- Vercel : 0€ (tier Hobby)
- Resend : 0€ (100 emails/jour gratuits)
- Domaine : 10€/an
- **Total : ~1€/mois**

**Phase croissance (10-50 clients) :**
- Supabase Pro : 25$/mois
- Vercel Pro : 20$/mois
- Resend : 20$/mois (10k emails)
- LemonSqueezy : 0€ (5% + frais transaction)
- Sentry : 0€ (tier gratuit)
- **Total : ~70$/mois (65€)**

**Phase scale (50-200 clients) :**
- Supabase : 25-100$/mois (usage)
- Vercel : 20$/mois
- Resend : 50$/mois
- Support : Crisp 25$/mois
- **Total : 120-200$/mois**

### 11.2 Temps de développement

**MVP (6 semaines) :**
- Développement : 30 jours × 6h = 180h
- Tests : 5 jours × 4h = 20h
- **Total : 200h**

**Si facturation freelance :**
- 200h × 50€/h (interne) = 10 000€
- ou 200h × 350€/h TJM (externe) = 70 000€

---

