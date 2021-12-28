# Ansible Role Duplicacy

install duplicacy autobackup via https://github.com/christophetd/duplicacy-autobackup

## Role Variables

| group | variable | default | description |
| --- | --- | ---| --- |
| basics | duplicacy_autobackup_image | `ghcr.io/christophetd/duplicacy-autobackup` | the image for duplicacy autobackup |
| basics | duplicacy_autobackup_image_tag | `v1.6.0` | the tag for `duplicacy_autobackup_image` |
| basics | duplicacy_autobackup_container_name | `duplicacy-autobackup` | the name for duplicacy autobackup container |
| backup | duplicacy_autobackup_backup_name | | the value for `BACKUP_NAME`, e.g. the name of the backup |
| backup | duplicacy_autobackup_backup_data | | the path to mount as `data`, e.g. the directory to backup |
| backup | duplicacy_autobackup_backup_encryption_key | | the value for `BACKUP_ENCRYPTION_KEY`, e.g.  the optional passphrase to encrypt teh backups with before they are stored remotely |
| backup | duplicacy_autobackup_backup_location | | the value for `BACKUP_LOCATION`, e.g. the [Duplicacy URI](https://github.com/gilbertchen/duplicacy/wiki/Storage-Backends) of where to store the backups |
| backup | duplicacy_autobackup_storage_provider  | | the storage provider, possible values are  <br /> <ol><li>`Backblaze B2`</li><li>`Onedrive`</li></ol> |
| backup | duplicacy_autobackup_token_file_path | `/srv/duplicacy-autobackup` | the path the token files are created |
| backup | duplicacy_autobackup_onedrive_token_file_name | `one-token.json`| the filename for `ONEDRIVE_TOKEN_FILE` |
| backup | duplicacy_autobackup_onedrive_token_file_content | | the content for `ONEDRIVE_TOKEN_FILE` |
| backup | duplicacy_autobackup_b2_id | | the value for `B2_ID` |
| backup | duplicacy_autobackup_b2_key | | the value for `B2_KEY` |
| backup | duplicacy_autobackup_backup_immediately | `no` | the value for `BACKUP_IMMEDIATELY`, e.g.  if a backup should be performed immediately after the container is started |
| backup | duplicacy_autobackup_backup_schedule | `0 1 * * *` | the value for `BACKUP_SCHEDULE`, e.g. cron-like string to define the frequency at which backups should be made (e.g. 0 2 * * * for Every day at 2am). Note that this string should be indicated in the UTC timezone. |
| prune | duplicacy_autobackup_prune_options | `-keep 365:3650 -keep 30:365 -keep 7:30 -keep 1:7 -a` | the value for `DUPLICACY_PRUNE_OPTIONS` |
| prune | duplicacy_autobackup_prune_schedule | `0 4 * * *` | the value for `PRUNE_SCHEDULE` |

## Ansible

The storage providers need different parameters and some storage providers need token files.
This by now is solved by a different task file for each provider.
