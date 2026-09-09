# Lowball

`NO TIME WASTERS, I KNOW WHAT I HAVE`

Lowball is here to solve the distinct problems that buyers and sellers have when exchanging physical goods to people they met on the internet.

--- Buyers problems

* Inexperience with product. If you are buyer getting into a new hobby, generally you don't know what to look for 
* Scammers. The item does not match the description
* Scammers. The seller insists on a 'holding fee' before buying the item (the item doesnt actually exist)

--- Sellers problems

* Crap descriptions. The seller not adding enough detail to the description. They might not know what detail to put in
* Mispricing. As a seller, you might not know how much your equivilent good is worth in the market so you either mark it too high (you get no interest) or too low (too much interest)
* Timewasters. People who send messages but have no real intention of buying
* People who don't read the ad. Buyers asking for information that is available within the ad if they read it
* Flakes. People saying that they are going to come and have a look at the thing you are selling but never come to look at it.
* Scammers. Some people try to scam you into holding an item for them or by showing a doctored invoce where they pay you too much and 'insist' you pay them back.
* Scammers. Some people try and scam you by insisting on using non-refundable internet transfers that get withdrawn after the sale

---

## 📦 Skills

Lowball ships with a skill for AI coding agents that do marketplace deal research.

### OpenCode skill

Location: `.opencode/skills/lowball-research/SKILL.md`

**Install:** Copy the entire `lowball-research/` directory into your OpenCode project's `.opencode/skills/` folder, or symlink it:

```bash
# In your project root
mkdir -p .opencode/skills
cp -r .opencode/skills/lowball-research .opencode/skills/lowball-research
```

### Hermes Agent skill

Location: `.hermes/skills/research/lowball/SKILL.md`

**Install:**

```bash
# One-time setup — copy the skill into Hermes' global skills directory
cp -r .hermes/skills/research/lowball ~/.hermes/skills/research/lowball
```

Or symlink to keep it in sync with the repo:

```bash
ln -sf "$(pwd)/.hermes/skills/research/lowball" ~/.hermes/skills/research/lowball
```

After installing, verify Hermes can see it:

```bash
hermes skills list | grep lowball
```

Expected output: `lowball — Marketplace deal-check and recommendation...`

### Using the skill

Once installed, any agent can load it by name. For example, to deal-check a Gumtree listing, the agent will automatically pick up the skill when the user asks "is this a good deal?" or mentions a marketplace listing.
