# Neovim in Ghostty for MailMate

A MailMate bundle that lets you edit email drafts in Neovim, launched inside [Ghostty](https://ghostty.org).

This bundle provides a single feature: it allows you to use Neovim inside Ghostty to compose new messages or reply to emails in MailMate. I built this bundle with some help from ChatGPT and Claude.

It adds a command to MailMate’s “Commands” menu that opens the message you’re composing or replying to in Neovim, launched inside Ghostty. Once you save and quit Neovim, the changes are synced back to MailMate automatically.

## Installation

If you downloaded the bundle as a ZIP file from GitHub, please rename the extracted folder from `neovim-ghostty.mmbundle-main` to `neovim-ghostty.mmBundle` before moving it to the Bundles directory.

To install the bundle:

1. Move the folder `neovim-ghostty.mmBundle` to `~/Library/Application Support/MailMate/Bundles`
2. Restart MailMate
3. After the restart, you should see a new option under the `Commands` menu: `Neovim (in Ghostty) > Edit`

### Uninstalling

To remove the bundle:

1. Delete the bundle from `~/Library/Application Support/MailMate/Bundles/`
2. Quit MailMate.
3. Delete the following files from `~/Library/Caches/com.freron.MailMate/`:
   - `BundlesIndex.binary`
   - `Cache.db`
   - `Cache.db-shm`
   - `Cache.db-wal`
4. Relaunch MailMate. The command `Neovim (in Ghostty) > Edit` should now be gone.

**Note:** These steps only apply to bundles you manually install in `~/Library/Application Support/MailMate/Bundles/`.

I learned this process from Benny Kjær Nielsen, the creator of MailMate. If you have further questions about MailMate bundles, I recommend reaching out to him directly, [searching the MailMate mailing list archives](http://www.mail-archive.com/mailmate@lists.freron.com/), or [posting a question on the mailing list.](https://lists.freron.com/listinfo/mailmate)

## How to Use

You can use this bundle to edit both new messages and replies.

### Composing a New Email

1. Create a new message in MailMate.
2. Choose `Commands > Neovim (in Ghostty) > Edit` from the menu bar, or press the keyboard shortcut "Shift + Control + O".
3. A new Ghostty terminal instance will open, launching Neovim with the email body loaded. This is a separate instance and won’t interfere with any existing Ghostty sessions you might have open.
4. After editing, use `:wq` to save and quit. The email body in MailMate will be updated automatically.

### Replying to an Email

1. Select a message and choose `Message > Reply...`.
2. When the MailMate composer opens, go to `Commands > Neovim (in Ghostty) > Edit`, or use the keyboard shortcut.
3. Follow the same editing process as above.

### Keyboard Shortcut

Instead of choosing the menu command manually, you can press: Shift + Control + O

This opens the current draft or reply in Neovim inside Ghostty.

## Notes

This bundle attempts to locate nvim using the PATH environment variable. If not found, it falls back to the default Homebrew installation path: `/opt/homebrew/bin/nvim` for Apple Silicon Macs and `/usr/local/bin/nvim` for Intel Macs. If Neovim is installed elsewhere, please update the path in the script manually.

If you installed Neovim elsewhere, either:

- Add its location to your `PATH`, or
- Edit `Support/bin/edit` to set the correct path manually.

Also make sure `Ghostty` is installed and accessible via `/usr/bin/open -na "Ghostty"`.

Finally, since Ghostty doesn’t support direct CLI launching on macOS, this bundle uses macOS’s `open -na "Ghostty"` command to launch it with arguments.[^1]

[^1]: I learned this from [this discussion.](https://github.com/ghostty-org/ghostty/discussions/3698)