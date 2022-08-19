# GCP Auth Github Action

This GitHub Action establishes authentication to Google Cloud. It supports
traditional authentication via a Google Cloud Service Account Key JSON and
authentication via [Workload Identity Federation][wif].

## Usage

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
      uses: 'docker/login-action@v1'
      with:
        registry: us-central1-docker.pkg.dev
        username: 'oauth2accesstoken'
        password: '${{ steps.auth.outputs.access_token }}'
```

[wif]: https://cloud.google.com/iam/docs/workload-identity-federation

