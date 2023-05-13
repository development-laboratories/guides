# Deploy via GitHub Action

- [GitHub SSH Key Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Create a new SSH key that will be used to deploy your updates:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-add --apple-use-keychain ~/.ssh/staging
```

Then add the `~/.ssh/staging` key to your repositories secrets named

- `SSH_DEPLOY_KEY`: `~/.ssh/staging` // the private key
- `SSH_DEPLOY_HOST`: `192.168.0.1` // remote servers ip
- `SSH_DEPLOY_USER`: `root` // or the user you will SSH as

Next add the following to a GitHub action:

```yaml
name: Deploy
on: [push, pull_request]
jobs:
  deploy:
    name: "Deploy to staging"
    runs-on: ubuntu-latest
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    steps:
      - name: Configure SSH
        run: |
          mkdir -p ~/.ssh/
          echo "$SSH_KEY" > ~/.ssh/staging.key
          chmod 600 ~/.ssh/staging.key
          cat >>~/.ssh/config <<END
          Host staging
            HostName $SSH_HOST
            User $SSH_USER
            IdentityFile ~/.ssh/staging.key
            StrictHostKeyChecking no
          END
        env:
          SSH_USER: ${{ secrets.DEPLOY_SSH_USER }}
          SSH_KEY: ${{ secrets.DEPLOY_SSH_KEY }}
          SSH_HOST: ${{ secrets.DEPLOY_SSH_HOST }}

      - name: Stop the server
        run: ssh staging 'cd ~/app && npm run deploy' # may vary based on project
```
