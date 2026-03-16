## General

General info and support for Rclone.

### Rclone DropBox & Nextcloud {#rclone_dropbox_nextcloud}

``` bash
rclone sync dropbox:/"Folder Name" nextcloud:/folder_name --verbose
# --dry-run for testing
```

## Example AI {#example_ai}

### Step 1: Installing rclone {#step_1_installing_rclone}

First, you need to make sure `rclone` is installed on your system:

1.  Open your terminal.
2.  Install rclone using your package manager. For Red Hat-based systems
    (like CentOS, Fedora, or RHEL), use: This will install rclone and
    any required dependencies.

### Step 2: Configure rclone for Dropbox {#step_2_configure_rclone_for_dropbox}

To set up Dropbox in `rclone`, follow these steps:

1.  Run `rclone config` to start the configuration wizard:
2.  Choose `n` for a new remote when prompted.
3.  Name the remote (e.g., `dropbox`).
4.  When asked to choose the storage type, scroll through the list and
    select Dropbox by typing the number next to it.
5.  Follow the prompts:
    - You might need to enter an API key if you have one, or you can
      leave it blank to use `rclone`\'s default.
    - A link will be provided to authorize `rclone` to access your
      Dropbox. Follow this link in your web browser, login to your
      Dropbox account, and allow access.
    - Once authorized, return to the terminal and proceed as directed.
6.  Finish and exit the configuration wizard.

### Step 3: Configure rclone for Nextcloud {#step_3_configure_rclone_for_nextcloud}

Nextcloud uses WebDAV, so the setup is slightly different:

1.  Run `rclone config` again to add another remote.
2.  Choose `n` for a new remote when prompted.
3.  Name the remote (e.g., `nextcloud`).
4.  For the storage type, select WebDAV by finding it in the list and
    entering its number.
5.  Enter the WebDAV URL for your Nextcloud instance: Replace
    `https://nextcloud.example.com` with your Nextcloud server\'s URL
    and `USERNAME` with your Nextcloud username.
6.  Choose `nextcloud` when asked for the type of WebDAV.
7.  Input your Nextcloud username and password.
8.  Finish and exit the configuration.

### Step 4: Sync Folder from Dropbox to Nextcloud {#step_4_sync_folder_from_dropbox_to_nextcloud}

Now that both remotes are configured, you can synchronize the folder:

1.  Identify the path of the folder in Dropbox you wish to sync, such as
    `/Photos`.
2.  Determine the path in Nextcloud where you want the folder to be
    synced, such as `/Backup`.
3.  Run the sync command: This command will sync the contents of
    `/Photos` from Dropbox to the `/Backup` directory in Nextcloud.
4.  Optionally, add the `--verbose` flag to see more details about the
    sync process:

### Step 5: Verify the Sync {#step_5_verify_the_sync}

It\'s a good practice to verify that the synchronization has been
completed successfully:

1.  Log in to your Nextcloud web interface.
2.  Navigate to the `/Backup` folder.
3.  Check that all files from the Dropbox `/Photos` folder are present
    and correct.
