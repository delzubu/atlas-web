---
title: Identity
parent: Core
grand_parent: Apps
nav_order: 2
---

# Identity

Identity provides sign-in across an Atlas instance. Use Account to manage your own settings. Instance administrators use the administration area to manage users and grant access to apps.

## How to use Identity

1. Sign in using a provider configured for your instance. If your identity is verified and needs approval, request an Atlas account and sign in again after approval.
2. Use **My account** to manage your displayed name and personal settings. If an app shows **Request access**, use that route to request the app entitlement.
3. Administrators use Identity’s administration area to manage users, app roles, requests and registered clients or modules within their authority.

App entitlements determine which actions you can take. Document apps also check Workspace access and the project grant. A reviewer may comment or participate in learning when the app supports it. Writers can create and edit content.

## Administration settings

Instance administrators open **Administration → Settings** to manage options that apply across an app. A value entered here overrides the deployment’s value. Credentials are masked. Leave a masked value in place to keep it, or enter a replacement and select **Save**. To clear an override, choose **Reset to the deployment’s value** or **Remove**, as shown. If another administrator saved while you were editing, reload and review their changes before saving again.

### Search settings

| Setting | What it controls |
| --- | --- |
| **Inference web address**, **Inference key header** and **Inference key** | The shared service that embeds queries and reranks results. The indexing CLI can also use it for document embeddings. |
| **The inference service tells passages from questions** | Enable this when the service supports separate passage and question encoding. E5 indexing needs this distinction. |
| **Embedding models a database may choose** and **Reranker models a database may choose** | Enter one model per line. A single listed model serves as the default. With several listed, a Search administrator can choose a model for each database in Search. |
| **Hugging Face token** and **OpenAI key** | Enable the optional Hugging Face embedder and the **Smart** search option, respectively. |

Database addresses, credentials, datasets and model selections are managed in Search by a user with the Search **admin** role. Instance administrators can grant that role in Identity. [Configure a shared database]({{ site.baseurl }}{% link docs/apps/search.md %}#configure-a-shared-database)
