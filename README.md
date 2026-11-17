# MyGymBro — legal pages

Static privacy policy, terms of use and account-deletion pages for the **MyGymBro** Android and
web app (repository `gym-spotter`). Published with GitHub Pages:

- https://danihp6.github.io/mygymbro-legal/
- https://danihp6.github.io/mygymbro-legal/privacidad.html (Spanish privacy policy)
- https://danihp6.github.io/mygymbro-legal/en/privacy.html (English privacy policy)

These are the URLs the Play Console listing and the app point at. The app reads them from
`frontend/src/app/core/legal.config.ts` in `gym-spotter`; changing a path here means changing that
constant too.

## Layout

```
index.html              hub, Spanish
privacidad.html         privacy policy, Spanish
terminos.html           terms of use, Spanish
eliminar-cuenta.html    account deletion, Spanish
en/index.html           hub, English
en/privacy.html         privacy policy, English
en/terms.html           terms of use, English
en/delete-account.html  account deletion, English
style.css               the only stylesheet
```

No build step, no Jekyll content, no external assets. `.nojekyll` keeps Pages from processing the
tree.

## Publishing

Pages serves the `main` branch root (`legacy` build). A push to `main` is the release: check the
four Spanish pages and the four English pages once they are live.

## What the pages say, and where it comes from

The content is written from what the product actually does, not from a template:

| Claim | Source of truth in `gym-spotter` |
| --- | --- |
| Identity data = Google email, name, picture | Cognito Google identity provider (`infra/lib/stacks/integration-stack.ts`) |
| Training data = exercises, sets, weights, repetitions, dates | `exercises` and `exercise-sessions` tables (`infra/lib/stacks/database-stack.ts`) |
| Premium = active flag, order id, hash of the purchase token only | `infra/src/lambdas/shared/premium.ts`, `verify-premium-purchase` |
| No analytics, no contacts, no location, no camera | no such dependency in `frontend/package.json` |
| Ads only on the free tier, served by AdMob | `frontend/src/app/core/services/ads.service.ts` |
| Processing in eu-west-1 | stack environment |

If the app starts collecting something new, this table is the checklist to update the policies.

## Open items

1. **Deletion request form.** The operator is creating a Google Form. When its URL exists, uncomment
   the marked block in `eliminar-cuenta.html` and `en/delete-account.html` and replace
   `REEMPLAZAR_CON_FORMULARIO` / `REPLACE_WITH_FORM`. Until then the pages state the email route,
   which works today (Play accepts an email-based request page, a form is the better experience).
2. **Ad consent (EEA).** The app does not ask for ad consent yet; the privacy page is written to stay
   true without it ("AdMob may use device identifiers… you can limit personalisation"). When the
   UMP consent flow ships in `gym-spotter`, add the sentence that consent is requested before
   personalised ads are served.
3. **Governing law** is stated as Spanish law with the consumer's own-country protection kept. Review
   if the operator ever trades from another jurisdiction.
4. **`app-ads.txt`** is already published at https://danihp6.github.io/app-ads.txt with publisher
   `pub-8140926326613612`; nothing to add here.

## Editing

Plain HTML, one `style.css`. Keep both languages in step: a change in `privacidad.html` has a
counterpart in `en/privacy.html`, and the same for the terms and the deletion page. Bump the
"last updated" date on the pages you touch.
