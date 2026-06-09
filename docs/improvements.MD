#   ReadME imrovements
- Simplfy README and add additional steps
- Create seperate Installation readme and FAQ
- 
#   Infra improvements
- Adding an option to not use TLS
- Adding an option to access mealie locally for those who release
- Adding Fail2Ban
- Adding LDAP subchart as option.


# Improve database handlement
Supply the posibility to add external database source.
``` yaml
externalDatabase
    enabled: True
  host: postgres-rw.database.svc
  port: 5432
  database: mealie
  username: mealie

  existingSecret: mealie-db-secret
```
``` yaml Example of how to use templating.
{{- define "mealie.database.host" -}}
{{- if .Values.postgresql.enabled -}}
{{ include "mealie.postgresql.fullname" . }}
{{- else -}}
{{ .Values.externalDatabase.host }}
{{- end -}}
{{- end -}}
``` 