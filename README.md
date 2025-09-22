# Chezmoi

```bash
sh -c "$(curl -fsLS get.chezmoi.io/lb)" -- init --apply GITHUBUSERNAME

chezmoi diff
chezmoi add ~/.vimrc
chezmoi edit ~/.vimrc


git add .
git commit -m "first commit"
git push

## https://www.chezmoi.io/user-guide/manage-machine-to-machine-differences/#use-templates
chezmoi data

chezmoi execute-template < ~/.local/share/chezmoi/home/.chezmoiexternal.toml.tmpl
chezmoi apply -v --refresh-externals

# nur .pub, unverschlüsselt
find ~/.ssh -maxdepth 1 -type f -name '*.pub' -print0 \
  | xargs -0 -r -I{} chezmoi add {}

# nur echte Private Keys, per Inhaltscheck
while IFS= read -r -d '' f; do
  if head -n1 "$f" | grep -qE '^(-----BEGIN (OPENSSH|RSA|DSA|EC|ED25519) PRIVATE KEY-----|openssh-key-v1)'; then
    chezmoi add --encrypt "$f"
  fi
done < <(find ~/.ssh -maxdepth 1 -type f ! -name '*.pub' -print0)
```
