09/02-2026
Breaking change:
- Alle variable names have been changed to camelCase!


16/07/2016
Change from 1.1.5 > 1.1.6
If you have an existing cluster where you make use of Longhorn and want to make use of the backups, you have to execute the following commands to enable reocurring backuping.
```bash
kubectl label pvc \
  -n mealie-production \
  mealie-api-data-claim-mealie-production \
  recurring-job.longhorn.io/source=enabled \
  recurring-job-group.longhorn.io/critical=enabled \
  --overwrite
```
```bash
kubectl label pvc \
  -n mealie-production \
  data-mealie-production-postgresql-0 \
  recurring-job.longhorn.io/source=enabled \
  recurring-job-group.longhorn.io/critical=enabled \
  --overwrite
  ```