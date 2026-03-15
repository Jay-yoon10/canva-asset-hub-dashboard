# Canva Asset Hub — Dashboard

> React dashboard for the [Canva Asset Hub](https://github.com/Jay-yoon10/canva-asset-hub) project — a live interface for managing S3 ↔ Canva asset synchronisation.

## Live Demo

**[https://jay-yoon10.github.io/canva-asset-hub-dashboard/](https://jay-yoon10.github.io/canva-asset-hub-dashboard/)**

> Login required — authenticated via Amazon Cognito (OAuth 2.0 PKCE)

---

## What it does

| Feature | Description |
|---------|-------------|
| **Asset List** | View all synced assets from DynamoDB — file name, sync direction, AI tags, status |
| **S3 → Canva Sync** | Manually trigger an upload from S3 to Canva by entering an S3 key |
| **Canva → S3 Export** | Export a Canva design back to S3 by entering a Design ID |
| **AI Tags** | View Bedrock-generated tags (`brand_tier`, `mood`) inline in the asset table |

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | React 18 (Vite) |
| Styling | Tailwind CSS v3 |
| Auth | AWS Amplify + Amazon Cognito (PKCE) |
| API | Amazon API Gateway (JWT authorised) |
| Deployment | GitHub Pages |

---

## Architecture

```
React (Vite)
    ↓
AWS Amplify — OAuth 2.0 PKCE → Amazon Cognito
    ↓
API Gateway (JWT authorizer)
    ↓
Lambda (canva-asset-hub-api)
    ↓
DynamoDB / S3 / Canva Connect API
```

Full architecture: [canva-asset-hub](https://github.com/Jay-yoon10/canva-asset-hub)

---

## Local Development

### Prerequisites
- Node.js 18+
- AWS Cognito User Pool configured (see [main repo setup guide](https://github.com/Jay-yoon10/canva-asset-hub#7-cognito-user-pool))

### Setup

```bash
git clone https://github.com/Jay-yoon10/canva-asset-hub-dashboard.git
cd canva-asset-hub-dashboard
npm install
```

Update `src/aws-config.js` with your Cognito and API Gateway values:

```js
const awsConfig = {
  Auth: {
    Cognito: {
      userPoolId: "your-user-pool-id",
      userPoolClientId: "your-client-id",
      loginWith: {
        oauth: {
          domain: "your-domain.auth.ap-southeast-2.amazoncognito.com",
          scopes: ["openid", "email", "phone"],
          redirectSignIn: ["http://localhost:5173"],
          redirectSignOut: ["http://localhost:5173"],
          responseType: "code",
        },
      },
    },
  },
};
```

Update `src/api.js` with your API Gateway URL:

```js
const API_BASE = "https://your-api-id.execute-api.ap-southeast-2.amazonaws.com/dev";
```

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

---

## Deployment (GitHub Pages)

```bash
npm run build
cp -r dist/* ../docs/
cd ..
git add .
git commit -m "deploy: update dashboard build"
git push
```

---

## Security Notes

This dashboard is built for portfolio/demonstration purposes.

| Area | This project | Production recommendation |
|------|-------------|--------------------------|
| JWT storage | Amplify-managed (memory + silent refresh) | BFF pattern with HttpOnly cookies |
| CORS | `Access-Control-Allow-Origin: *` | Restrict to specific origin |
| Cognito scopes | openid, email, phone | Minimum required scopes only |

---

## Related

- [canva-asset-hub](https://github.com/Jay-yoon10/canva-asset-hub) — Main repo (Lambda, API Gateway, DynamoDB, Bedrock)
- [Portfolio Site](https://jay-yoon10.github.io/canva-asset-hub/) — Project documentation

---

## Author

**Jay (Yeojoon) Yoon** — Cloud Engineer at AWS
[LinkedIn](https://www.linkedin.com/in/jay-yoon-0294801b1/) · [GitHub](https://github.com/Jay-yoon10)

> *Built as a portfolio project. Views are my own and not those of my employer.*