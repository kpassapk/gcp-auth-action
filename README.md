# GCP Auth Github Action

This GitHub Action establishes authentication to Google Cloud with [Workload
Identity Federation][wif]. It is a thin wrapper on top of the Google
[secrets][act], applying conventions used at Yalo.

## Usage

1. Add your repository to the repository [list][tf] and send a pull request.
This will create the service account that your repository will use to
authenticate.

2. Add a step, following this example:


```yaml
jobs:
  job_id:
    # Add "id-token" with the intended permissions.
    permissions:
      contents: 'read'
      id-token: 'write'

    steps:
    # actions/checkout MUST come before auth
    - uses: 'actions/checkout@v3'

    - id: 'auth'
      name: 'Authenticate to Google Cloud'
      uses: kpassapk/gcp-auth-action@v0'
    - name: Login to Artifact Registry
      uses: 'docker/login-action@v2'
      with:
        registry: us-central1-docker.pkg.dev
        username: 'oauth2accesstoken'
        password: '${{ steps.auth.outputs.access_token }}'
```

[tf]: https://bitbucket.org/yalochat/yalo-gcp-iam-production/src/master/yalo-production-env/github-oidc.tf
[wif]: https://cloud.google.com/iam/docs/workload-identity-federation
[act]: https://github.com/google-github-actions/auth
