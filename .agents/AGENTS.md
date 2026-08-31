# Workspace Rules for 開発したAI

Every time a new task or feature development starts, you MUST proactively remind the user of all available custom skills (both global and workspace-scoped) and ask if they should be applied.

## All Available Skills

1. **`vercel-kv-rate-limiting`** (Workspace): Use this when building APIs or pages that require trial rate limits based on IP/User (supports 72-hour reset or permanent limit patterns).
2. **`brand-pwa-cross-promotion`** (Workspace): Use this when modifying headers, introducing cross-service banner links, or updating app/PWA icons and manifests.
3. **`stripe-self-serve-cancellation`** (Workspace): Use this when setting up automated client-side localStorage billing states or redirecting users to the Stripe Customer Portal for self-serve cancellation.
4. **`ai-image-processing-core`** (Global): Use this for Gemini API configurations, image generation prompts, style transfer, or general image processing logic.
5. **`blog-seo-integration`** (Global): Use this for SEO optimizations, blog generation logic, RSS feed configurations, or Search Console verification setup.
6. **`premium-ui-design`** (Global): Use this for high-quality UI design tokens, animations, pricing cards, responsive layout structures, and sleek modern web designs.

---

## Action Item for Agents
At the beginning of any implementation phase, plan discussion, or task initialization, explicitly output a checklist of these skills and ask:
- "Would you like to apply any of the following available skills to this task?"
  - [ ] `vercel-kv-rate-limiting` (Vercel KV制限: 72時間リセット/永久制限などの回数管理)
  - [ ] `brand-pwa-cross-promotion` (相互バナー/PWA設定: 姉妹サイト誘導バナーやPWA設定)
  - [ ] `stripe-self-serve-cancellation` (Stripe自動解約: StripeポータルやlocalStorageによるセルフ解約ボタン)
  - [ ] `ai-image-processing-core` (Gemini画像処理: Gemini API設定や画像生成プロンプト)
  - [ ] `blog-seo-integration` (SEOブログ連携: SEO対策やブログ生成・検索コンソール)
  - [ ] `premium-ui-design` (Premium UIデザイン: アニメーションやモダンな配色・価格カード)
