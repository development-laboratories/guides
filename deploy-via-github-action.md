# Deploy via GitHub Action

- [GitHub SSH Key Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## 1. Create a new SSH key name `deploy.pub` that will be used to deploy update from GitHub to your server

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## 2. Copy keys to server

**Important** make sure to now copy the `<your_key>.pub` to the remote host via
```bash
ssh-copy-id -i ~/.ssh/<your_key>.pub YOUR_USER_NAME@IP_ADDRESS_OF_THE_SERVER
```

## 3. Create GitHub secrets 

Copy the all of the text from your `~/.ssh/deploy` (including the `-- BEGIN / KEY --`) by running

```bash
# copy to clipboard
pbcopy < ~/.ssh/soladex_deploy
```
or
```bash
# echo to terminal
cat < ~/.ssh/soladex_deploy
```

```
-----BEGIN OPENSSH PRIVATE KEY-----
...        <omitted>
-----END OPENSSH PRIVATE KEY-----
```

|secret|value|description|
|SSH_DEPLOY_KEY| <~/.ssh/deploy|
|SSH_DEPLOY_HOST| 198.199.98.58|
|SSH_DEPLOY_USER| root|
```

<img width="1329" alt="Screenshot 2023-05-13 at 4 41 36 PM" src="https://github.com/development-laboratories/guides/assets/10716803/fb6d72b4-f3d4-4227-a0d0-2ffdd56cbda8">


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
          echo "$SSH_KEY" > ~/.ssh/deploy.key
          chmod 600 ~/.ssh/deploy.key
          cat >>~/.ssh/config <<END
          Host deploy
            HostName $SSH_HOST
            User $SSH_USER
            IdentityFile ~/.ssh/deploy.key
            StrictHostKeyChecking no
          END
        env:
          SSH_USER: ${{ secrets.DEPLOY_SSH_USER }}
          SSH_KEY: ${{ secrets.DEPLOY_SSH_KEY }}
          SSH_HOST: ${{ secrets.DEPLOY_SSH_HOST }}

      - name: Stop the server
        run: ssh staging 'cd ~/app && npm run deploy' # may vary based on project
```

## Before Running the Deploy

After adding the new **SSH** keys to your server and GitHub account(s) make sure to do a test run.

- Manually SSH into your server and run the deployment
- Agree and accept to adding to "known hosts"

## Add to personal GitHub SSH Keys (Optional)

If you want to be able to download from GitHub via your custom deploy script, then you may need to add this to your https://github.com/settings/profile

## Troubleshooting

- GitHub action is failing on `SSH` step: make sure that you have properly copied the keys to your server.

