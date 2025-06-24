# InSpec von GitHub Packages verwenden

## 1. Fork und Workflow einrichten

1. Forke das InSpec Repository zu deinem GitHub Account
2. Die Workflow-Datei `.github/workflows/build-and-publish-gem.yml` ist bereits vorhanden
3. Der Workflow läuft automatisch bei:
   - Push auf `main` Branch
   - Manueller Auslösung über GitHub Actions UI
   - Tags die mit `v` beginnen

## 2. Gems in deinem Projekt verwenden

### Option A: Mit Bundler (empfohlen)

```ruby
# Gemfile
source "https://rubygems.org"

# Ersetze YOUR_GITHUB_USERNAME mit deinem GitHub Benutzernamen (lowercase!)
source "https://rubygems.pkg.github.com/YOUR_GITHUB_USERNAME" do
  gem "inspec-core", "~> 6.8"
  # oder für die volle Version:
  # gem "inspec", "~> 6.8"
end
```

Dann authentifizieren:

```bash
# Lokal entwickeln
export BUNDLE_RUBYGEMS__PKG__GITHUB__COM="YOUR_GITHUB_TOKEN:x-oauth-basic"
bundle install

# Oder mit bundle config
bundle config https://rubygems.pkg.github.com/YOUR_GITHUB_USERNAME YOUR_GITHUB_TOKEN
```

### Option B: In GitHub Actions

```yaml
# .github/workflows/your-workflow.yml
- name: Configure Bundler for GitHub Packages
  run: |
    bundle config https://rubygems.pkg.github.com/${{ github.repository_owner }} \
      ${{ secrets.GITHUB_TOKEN }}

- name: Install dependencies
  run: bundle install
```

### Option C: Direkte Installation

```bash
gem install inspec-core \
  --source https://YOUR_GITHUB_TOKEN@rubygems.pkg.github.com/YOUR_GITHUB_USERNAME
```

## 3. Authentifizierung

### Personal Access Token erstellen

1. Gehe zu GitHub Settings → Developer settings → Personal access tokens
2. Erstelle einen neuen Token mit diesen Berechtigungen:
   - `read:packages` (zum Installieren)
   - `write:packages` (zum Publishen, nur im Fork benötigt)
   - `repo` (für private Repositories)

### Token in verschiedenen Umgebungen nutzen

**Lokal (.bashrc/.zshrc):**
```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxxxxxxxxx"
export BUNDLE_RUBYGEMS__PKG__GITHUB__COM="${GITHUB_TOKEN}:x-oauth-basic"
```

**Docker:**
```dockerfile
ARG GITHUB_TOKEN
RUN bundle config https://rubygems.pkg.github.com/YOUR_GITHUB_USERNAME ${GITHUB_TOKEN}
```

**CI/CD:**
- Speichere Token als Secret in deinem Repository
- Nutze `${{ secrets.GITHUB_TOKEN }}` (automatisch in GitHub Actions)

## 4. Versionierung

Der Workflow nutzt die VERSION Datei aus dem InSpec Repository. Du kannst:

1. Die VERSION Datei anpassen und pushen
2. Einen Tag erstellen: `git tag v6.8.38-custom && git push --tags`
3. Manuell über GitHub Actions UI auslösen

## 5. Troubleshooting

**401 Unauthorized:**
- Token hat nicht die richtigen Berechtigungen
- Token ist abgelaufen
- Benutzername muss lowercase sein

**404 Not Found:**
- Package wurde noch nicht gepublisht
- Falscher Benutzername/Organisation
- Private Package ohne Leserechte

**Gem nicht gefunden:**
```bash
# Liste alle verfügbaren Versionen
gem list --remote --all inspec-core \
  --source https://rubygems.pkg.github.com/YOUR_GITHUB_USERNAME
```

## 6. Alternative: Lokaler Gem Server

Falls GitHub Packages nicht funktioniert:

```bash
# Gems lokal bauen
gem build inspec-core.gemspec
gem build inspec.gemspec

# Lokalen Gem Server starten
gem install geminabox
gem inabox inspec-*.gem
```