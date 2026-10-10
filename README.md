# Wheels Through History - Telegram/Facebook hourly bot (Vehicles)

pch-র মতোই কাজ করে, শুধু বিষয় Vehicles (গাড়ি, ট্রেন, জাহাজ, প্লেন, বাইক, রেস, রেকর্ড)।
ছবি বানায় **Cloudflare Workers AI** এর `@cf/black-forest-labs/flux-2-klein-4b` দিয়ে (দৈনিক ১০,০০০ neuron ফ্রি)।

## সেটআপ
1. PRIVATE GitHub repo খুলে এই ফোল্ডারের সব ফাইল আপলোড করুন (`.github` ফোল্ডারসহ)।
2. Cloudflare:
   - Dashboard > Workers AI থেকে **Account ID** নিন।
   - Workers AI > Use REST API > **Create a Workers AI API Token** দিয়ে টোকেন বানান।
3. Telegram: @BotFather থেকে বট বানিয়ে চ্যানেলে ADMIN করুন, চ্যানেল id নিন (`-100...`)।
4. OpenRouter keys: আগের মতোই (একাধিক হলে লাইন বা কমা দিয়ে আলাদা)।
5. Repo > Settings > Secrets and variables > Actions:
   - Secrets: `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_API_TOKEN`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, `OPENROUTER_API_KEYS`
   - (ঐচ্ছিক) `FB_PAGE_ID`, `FB_PAGE_TOKEN`
   - Variables (ঐচ্ছিক): `CF_IMAGE_MODEL`, `GEN_SIZE`, `IMAGE_CANDIDATES`, `PAGE_NAME`
6. Actions > hourly-post > Run workflow দিয়ে একবার টেস্ট করুন। ট্রিগার আগের মতো cron-job.org (workflow_dispatch API)।

## ফ্রি কোটা (১০k neuron/দিন)
- মডেল আউটপুট ৫১২x৫১২ টাইল হিসেবে চার্জ হয়। ডিফল্ট `GEN_SIZE=768x960` (৪ টাইল, আনুমানিক ~১০০ neuron/ছবি), তারপর 1080x1350-তে বড় করা হয়।
- `IMAGE_CANDIDATES=2` ডিফল্ট: ঘণ্টায় ২টা ছবি চেষ্টা = দিনে ~৪৮টা ≈ ৫k neuron। বাড়ালে খরচ বাড়বে।
- কোটা শেষ হলে বট নিজে থেকে থেমে একটা টেক্সট-কার্ড ছবি দিয়ে পোস্ট করে; কোটা রিসেট হয় প্রতিদিন 00:00 UTC-তে।
- সঠিক খরচ Cloudflare Dashboard > Workers AI > Usage-এ দেখে নিন।

## লোকাল টেস্ট
env সেট করুন: `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_API_TOKEN`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, `OPENROUTER_API_KEYS`, তারপর `python bot.py`
