# Backup & Restore

## Summary

Sometime you need to backup and/or restore your instance of Organizr.

## Backing Up Organizr

Head on over to the Backup module of Organizr

{% hint style="info" %}
Settings / System Settings / Backup
{% endhint %}

All that you need to do is click the `Create Backup Button`

![](<../.gitbook/assets/image (31).png>)

Once the backup process completes, the new back up will be listed at the bottom of the back up listing.  There are 2 buttons that are accessible for each backup.

The left button is the download button which will download the zip file that includes all the backed up files.  The right button will delete that specific backup zip file.

## Restoring Organizr

{% hint style="warning" %}
As of Version `2.1.500` the restore process has not been written for Organizr.
{% endhint %}

### Manual Restore Process

1. Install Organizr as you would normally
2. Unzip the backup zip file somewhere
   1. Inside the zip file you will find the folder structure of where the files need to be placed.
      1. If you placed the database-folder elsewhere, you need to update the `'dbLocation' => 'dbPath'` value in `config.php`
3. Restart the docker (or process if not in docker) and refresh the web page for Organizr
