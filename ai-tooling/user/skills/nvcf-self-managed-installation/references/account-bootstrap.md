# Verify the account bootstrap ran

The default account is created by a `post-install` hook on the `api` release
(`job/nvcf-api-account-bootstrap` in `nvcf`). Because it is a post-install hook, it only
fires on a successful `api` install. If that install failed and you repaired it with live
patches (rather than a clean re-sync), the hook never runs, no account is created, and
every subsequent function/account call returns a cryptic `404 Unknown client_id` with
nothing obviously wrong in the running pods.

Verify it ran after the first sync:

```bash
kubectl get job -n nvcf nvcf-api-account-bootstrap \
  -o jsonpath='{.status.succeeded}'   # expect 1
```

If the job is absent or `0`, re-run it by re-syncing the `api` release so the hook fires
again (delete the prior job first if it lingers in a completed/failed state and blocks the
hook):

```bash
kubectl delete job -n nvcf nvcf-api-account-bootstrap --ignore-not-found
HELMFILE_ENV=<env> helmfile --selector name=api sync
kubectl logs -n nvcf job/nvcf-api-account-bootstrap   # confirm the account was created
```
