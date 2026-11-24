---
title: Connect a local Neo4j database
nav_exclude: true
permalink: /docs/apps/search/connect-local-neo4j/
---

# Connect a Neo4j on your computer

This guide takes a Neo4j database on your own computer (a code index, any graph) to the point where Search can read it, step by step.
Search runs in the cloud, so it cannot reach `localhost`: you put a public **https** address in front of the database with a Cloudflare
tunnel, and connect that address as one of your own databases. Only you see it, Search only reads it, and it goes
a day after you last used it.

Steps 1 to 5 are all you need. Step 6 adds search by meaning, step 7 a stable address protected by a token.

## 1. What you need

- **Neo4j 5.19 or later.** Search talks to Neo4j's HTTP Query API, which arrived in 5.19. It is on by default from 5.25; on 5.19 to 5.24
  you switch it on in step 2.
- The **user and password** of the database.
- **cloudflared**, Cloudflare's tunnel program (step 4). A quick tunnel needs no Cloudflare account.
- The Search **writer** role: Your databases is shown only to writers.

## 2. Prepare Neo4j

Choose the way your Neo4j runs.

### Neo4j Desktop or the Windows zip

1. Open the configuration file `neo4j.conf`:
   - **Neo4j Desktop**: in the **Open** menu of the database, choose **Terminal**, then run `cd conf`; the file is in that folder.
   - **Zip or tarball**: `<the folder you extracted>\conf\neo4j.conf`.
2. Check these lines. Add the ones that are missing; leave the others as they are.

   ```
   server.http.enabled=true
   server.http.listen_address=:7474
   db.transaction.timeout=30s
   ```

   `db.transaction.timeout` makes Neo4j stop a statement after 30 seconds, as Search does. From Neo4j 2026.04 Search also asks the database to stop
   each statement after 30 seconds itself; on older versions this line is the only way, and without it a query Search gave up on keeps running
   in the database.
3. **Neo4j 5.19 to 5.24 only**: switch the Query API on. Add `QUERY_API_ENDPOINTS` to the HTTP modules:

   ```
   server.http_enabled_modules=TRANSACTIONAL_ENDPOINTS,UNMANAGED_EXTENSIONS,BROWSER,ENTERPRISE_MANAGEMENT_ENDPOINTS,QUERY_API_ENDPOINTS
   ```

4. Restart the database.

### Docker, or Podman

The image takes its settings as environment variables: `NEO4J_` in front, every `.` written as `_`, and every `_` written as `__`.
On Windows run this in PowerShell; on macOS or Linux in a terminal. Put your own password in place of `your-password`, and use the
Neo4j version you want in place of the tag.

```powershell
docker run --detach --name neo4j-search --publish=7474:7474 --publish=7687:7687 --volume=$HOME/neo4j/data:/data --env NEO4J_AUTH=neo4j/your-password --env NEO4J_db_transaction_timeout=30s neo4j:2026.10.2
```

**Podman** takes the same arguments; give the image its full name:

```powershell
podman run --detach --name neo4j-search --publish=7474:7474 --publish=7687:7687 --volume=$HOME/neo4j/data:/data --env NEO4J_AUTH=neo4j/your-password --env NEO4J_db_transaction_timeout=30s docker.io/library/neo4j:2026.10.2
```

On Windows and macOS, Podman runs containers in a virtual machine: start it first with `podman machine start`.

For an image from 5.19 to 5.24, also add `--env NEO4J_server_http__enabled__modules=TRANSACTIONAL_ENDPOINTS,UNMANAGED_EXTENSIONS,BROWSER,ENTERPRISE_MANAGEMENT_ENDPOINTS,QUERY_API_ENDPOINTS`.

Your data stays in `$HOME/neo4j/data` when the container is removed.

### macOS or Linux, installed without containers

Neo4j reads the same `neo4j.conf` (in the `conf` folder of the installation). Make the same changes as for the Windows zip above, then restart it.

### A read-only user (Enterprise Edition)

Search sends only read statements and refuses people's statements that could write, but over https Neo4j has no read-only mode Search can
switch on: the safest connection is one whose user cannot write at all. In **Enterprise Edition**, run this in Neo4j Browser
against the `system` database (`:use system`), with a password of your own:

```cypher
CREATE USER search_reader SET PASSWORD 'a-long-password' CHANGE NOT REQUIRED
```

```cypher
GRANT ROLE reader TO search_reader
```

Then use `search_reader` in step 5. **Community Edition** has no roles: every user can do everything. Use a strong password there, and keep
the tunnel running only while you need it (or protect it with step 7).

## 3. Check it on your computer

Ask the Query API for `RETURN 1` before any tunnel is involved.

**Windows (PowerShell):**

```powershell
$auth = [Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("neo4j:your-password"))
Invoke-RestMethod -Method Post -Uri http://localhost:7474/db/neo4j/query/v2 -Headers @{ Authorization = "Basic $auth" } -ContentType "application/json" -Body '{"statement": "RETURN 1 AS ok"}'
```

**macOS or Linux:**

```bash
curl -u neo4j:your-password -H "Content-Type: application/json" -d '{"statement": "RETURN 1 AS ok"}' http://localhost:7474/db/neo4j/query/v2
```

A working database answers with `data` holding `fields` `["ok"]` and `values` `[[1]]`. If your database is not called `neo4j`, put its name
in place of `neo4j` after `/db/`.

- **404**: the Query API is off (5.19 to 5.24: step 2) or Neo4j is older than 5.19.
- **401**: the user or password is wrong.
- **Nothing answers**: Neo4j is not running, or HTTP is on another port.

## 4. Open a quick tunnel

1. Install cloudflared:
   - **Windows**: `winget install -e --id Cloudflare.cloudflared` (or the `.msi` from Cloudflare's downloads page).
   - **macOS**: `brew install cloudflared`.
   - **Debian or Ubuntu**: add Cloudflare's package repository and install it:

     ```bash
     sudo mkdir -p --mode=0755 /usr/share/keyrings
     curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
     echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
     sudo apt-get update && sudo apt-get install cloudflared
     ```

   Check it with `cloudflared --version`.
2. Start the tunnel to the database's HTTP port, and leave the window open:

   ```powershell
   cloudflared tunnel --url http://localhost:7474
   ```

3. cloudflared prints an address on `trycloudflare.com`, such as `https://some-random-words.trycloudflare.com`. That is the database's
   address for Search. Run the check of step 3 again with it in place of `http://localhost:7474`.

A quick tunnel is for testing: it has a new address every time you start it, stops when you close the window, and has no uptime guarantee.
When you start it again, reconnect the database with its new address: in **Your databases**, the pencil next to it (or **Edit database…** on its
folder), paste the address, **Save**. An agent does the same with `search_update_connection`.
That suits a connection that lasts a day. For an address that stays the same, see step 7.

## 5. Connect it in Search

1. In Search, open **Your databases** (the tool at the top of the navigator) and choose **Connect a database…**.
2. Fill in:
   - **Name**: what you will see in the navigator, for example `Laptop code graph`.
   - **Database address**: the `https://….trycloudflare.com` address from step 4.
   - **User** and **Password**: those of step 2 (or `search_reader`).
   - **Database name**: only when it is not `neo4j`.
   - Leave the access and embedding fields empty for now.
3. Press **Connect**. Search tries the database before it saves anything:
   - **unreachable**: the tunnel is not running, the computer is asleep, or the address is mistyped.
   - **unauthorized**: the user or password is wrong.
   - **refused**: a login page answered instead of the database (an Access-protected address without its token: step 7).
   - **bad_answer**: something other than Neo4j's Query API answered (a wrong port, or Neo4j older than 5.19).
4. The database appears in the navigator, marked **temporary**, with one index, **default**: the whole database. Open
   Statistics on it to see its labels, relationships and indexes, then ask it in Native query.

An agent can do the same over MCP with `search_add_connection`.

## 6. Search by meaning (optional)

If your graph has **vector indexes**, Search can search them by meaning. It must turn your question into a vector with **the same model that
built the index**, so it needs to know the model and have something that runs it. An **embedding endpoint** on your computer does that for free.
Ollama is the example here; any server that speaks the OpenAI-compatible `/v1/embeddings` (LM Studio, Hugging Face TEI, your own code) works the same way.

1. Install Ollama and download the model your index was built with, for example:

   ```powershell
   ollama pull nomic-embed-text
   ```

   Ollama listens on `127.0.0.1` port 11434.
2. Open a second quick tunnel, to Ollama. Ollama's own instructions for tunnels pass the `Host` header as `localhost:11434`; do the same:

   ```powershell
   cloudflared tunnel --url http://localhost:11434 --http-host-header="localhost:11434"
   ```

3. Check it through the tunnel (macOS or Linux shown; in PowerShell use `Invoke-RestMethod` as in step 3):

   ```bash
   curl -H "Content-Type: application/json" -d '{"model": "nomic-embed-text", "input": "read the settings file"}' https://other-random-words.trycloudflare.com/v1/embeddings
   ```

   A list of numbers in the answer means it works.
4. In Search, connect the database (remove and connect it again if it is already there) with, in addition:
   - **Embedding model**: the model, for example `nomic-embed-text`. If one vector index was built with another model, add a line
     `index-name=model` under **Other models**.
   - **Embedding endpoint**: the Ollama tunnel's address followed by `/v1`, for example `https://other-random-words.trycloudflare.com/v1`.
   - **Query embedders**: leave it at `endpoint`.

In Native query choose **Vector**: Search shows which model will embed your text. A model that makes vectors of another size than the index
is refused, because it is not the one that built it. Without an endpoint, an agent can still send a vector it made itself. Search can also use
`hf-inference` (Hugging Face, paid by your deployment) if you add it to Query embedders, for models published on Hugging Face.

## 7. A stable, protected address (optional, later)

A quick tunnel changes its address every time, and anyone who learns the address can try your password. A **named tunnel** on a domain of
your own keeps the same address, and **Cloudflare Access** with a **service token** keeps everyone else out. You need a Cloudflare account and a
domain whose name servers are Cloudflare's.

1. Sign in and create the tunnel:

   ```powershell
   cloudflared tunnel login
   cloudflared tunnel create neo4j-search
   ```

   `create` prints the tunnel's id and where it wrote its credentials file.
2. Write `config.yml` in the `.cloudflared` folder in your home directory (on Windows `%USERPROFILE%\.cloudflared`):

   ```yaml
   tunnel: <tunnel id>
   credentials-file: <the credentials file create printed>
   ingress:
     - hostname: graph.example.net
       service: http://localhost:7474
     - service: http_status:404
   ```

   The last line is required: it answers everything that matches no hostname. To expose Ollama on the same tunnel, add before it a second
   hostname with `service: http://localhost:11434` and, under it, `originRequest:` with `httpHostHeader: localhost:11434`.
3. Point the hostname at the tunnel and run it:

   ```powershell
   cloudflared tunnel route dns neo4j-search graph.example.net
   cloudflared tunnel run neo4j-search
   ```

4. Protect it with Access. In Cloudflare **Zero Trust**:
   - create a **service token** (Access controls, Service credentials, Service Tokens). Copy the client id and the client secret: the
     secret is shown only once.
   - add an **Access application** for `graph.example.net` with a policy whose action is **Service Auth** and which includes that token.
     With any other action Access shows a login page, and Search reports **refused**.
5. In Search, connect `https://graph.example.net` and fill in **Access client id** and **Access client secret**. Search sends them as
   `CF-Access-Client-Id` and `CF-Access-Client-Secret` with every request.

## Troubleshooting

| What you see | Why | What to do |
|---|---|---|
| **refused** when connecting | Cloudflare Access answered with a login page | Give the Access client id and secret, and check the policy's action is Service Auth |
| **unreachable** | The tunnel is stopped, the computer sleeps, or the quick-tunnel address changed | Start the tunnel; edit the database and paste its new address |
| **bad_answer** | Not Neo4j's Query API at that address | Check step 3 through the tunnel; check the port and the Neo4j version |
| A query stops after 30 seconds | Search's limit | Add a `LIMIT`, narrow the statement; set `db.transaction.timeout` so the database stops too |
| Vector search refused: no model | No embedding model on the connection | Connect it again with Embedding model (step 6) |
| Vector search refused: another size | The model is not the one that built the index | Use the index's own model |
| Vector search: the endpoint cannot be reached | The Ollama tunnel is stopped, or it lacks `--http-host-header` | Start it as in step 6 |
| The database has gone from Search | A day without use passed | Connect it again |
