# Ansible Role Duplicacy

install duplicacy autobackup via https://github.com/christophetd/duplicacy-autobackup

## Role Variables

| group | variable | default | description |
| --- | --- | ---| --- |
| basics | duplicacy_autobackup_image | `ghcr.io/christophetd/duplicacy-autobackup` | the image for duplicacy autobackup |
| basics | duplicacy_autobackup_image_tag | `v1.5.0` | the tag for `duplicacy_autobackup_image` |
| basics | duplicacy_autobackup_container_name | `duplicacy-autobackup` | the name for duplicacy autobackup container |
| backup | duplicacy_autobackup_backup_name | | the value for `BACKUP_NAME`, e.g. the name of the backup |
| backup | duplicacy_autobackup_working_directory | | the path to mount as `data`, e.g. the working directory for duplicacy which is the default path for the repository to backup |
| backup | duplicacy_autobackup_backup_encryption_key | | the value for `BACKUP_ENCRYPTION_KEY`, e.g.  the passphrase to encrypt teh backups with before they are stored remotely |
| backup | duplicacy_autobackup_backup_location | | the value for `BACKUP_LOCATION`, e.g. the [Duplicacy URI](https://github.com/gilbertchen/duplicacy/wiki/Storage-Backends) of where to store the backups |
| backup | duplicacy_autobackup_storage_backend | | the storage backend, possible values are  <br /><ol><li>`Local disk`</li><li>`Backblaze B2`</li><li>`SSH/SFTP Password`</li><li>`SSH/SFTP Keyfile`</li><li>`Onedrive`</li></ol> |
| backup | duplicacy_autobackup_duplicacy_init_options | `''` | the value for `DUPLICACY_INIT_OPTIONS`, e.g. the options for `duplicacy init` |
| backup | duplicacy_autobackup_repository | `"{{ duplicacy_autobackup_working_directory }}"` | the path mounted as `/repository` for possible use in `duplicacy_autobackup_duplicacy_init_options` |
| backup | duplicacy_autobackup_secrets_file_path | `/srv/duplicacy-autobackup/secrets` | the path where the token and the ssh-key files are created |
| backup | duplicacy_autobackup_secret_file_name | it depends on `duplicacy_autobackup_storage_backend` | the filename for the secret file, the default is <br /><ol><li>`Local disk`</li><li>`Backblaze B2`</li><li>`SSH/SFTP Password`</li><li>`SSH/SFTP Keyfile`<br />`{{ duplicacy_autobackup_ssh_key_file_name }}`</li><li>`Onedrive`<br />`{{ duplicacy_autobackup_onedrive_token_file_name }}`</li></ol> |
| backup | duplicacy_autobackup_secret_file_content | | the content for `duplicacy_autobackup_secret_file_name` |
| backup | duplicacy_autobackup_onedrive_token_file_name | `one-token.json`| the filename for `ONEDRIVE_TOKEN_FILE` |
| backup | duplicacy_autobackup_ssh_key_file_name | `id` | the filename for `SSH_KEY_FILE` |
| backup | duplicacy_autobackup_b2_id | | the value for `B2_ID` |
| backup | duplicacy_autobackup_b2_key | | the value for `B2_KEY` |
| backup | duplicacy_autobackup_backup_immediately | `'no'` | the value for `BACKUP_IMMEDIATELY`, e.g. if a backup should be performed immediately after the container is started (`'yes'` or `'no'`) |
| backup | duplicacy_autobackup_backup_schedule | `0 1 * * *` | the value for `BACKUP_SCHEDULE`, e.g. cron-like string to define the frequency at which backups should be made (e.g. 0 2 * * * for Every day at 2am). Note that this string should be indicated in the UTC timezone. |
| backup | duplicati_autobackup_scriptfile_path | `/srv/duplicacy-autobackup/scripts` | the path where the scripts are create |
| backup | duplicacy_autobackup_pre_backup_script_file_name | `pre-backup.sh` | the filename for the pre backum script |
| backup | duplicacy_autobackup_pre_backup_script_file_content |  | the content for `/scripts/pre-backup.sh` |
| backup | duplicacy_autobackup_post_backup_script_file_name | `post-backup.sh` | the filename for the post backum script |
| backup | duplicacy_autobackup_post_backup_script_file_content |  | the content for `/scripts/post-backup.sh` |
| prune | duplicacy_autobackup_prune_options | `-keep 365:3650 -keep 30:365 -keep 7:30 -keep 1:7 -a` | the value for `DUPLICACY_PRUNE_OPTIONS` |
| prune | duplicacy_autobackup_prune_schedule | `0 4 * * *` | the value for `PRUNE_SCHEDULE` |

## Test

This role can be tested by
```bash
ansible-playbook test/playbook.yml -e "test_backend='Not implemented'"
ansible-playbook test/playbook.yml -e "test_backend='Local disk'"
ansible-playbook test/playbook.yml -e "test_backend='Blackblaze B2'" -e@test/.blackblaze_b2.yml
ansible-playbook test/playbook.yml -e "test_backend='Onedrive'" -e@test/.onedrive.yml
ansible-playbook test/playbook.yml -e "test_backend='SSH/SFTP Keyfile'" -e@test/.ssh_ftp_key.yml
```
