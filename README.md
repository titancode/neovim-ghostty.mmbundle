# Neovim (in Ghostty) MailMate Bundle

A MailMate bundle that lets you edit email drafts in Neovim, launched inside [Ghostty](https://ghostty.org).

This bundle provides a single feature: it allows you to use Neovim inside Ghostty to compose new messages or reply to emails in MailMate. I built this bundle with some help from ChatGPT and Claude.

It adds a command to MailMate’s “Commands” menu that opens the message you’re composing or replying to in Neovim, launched inside Ghostty. Once you save and quit Neovim, the changes are synced back to MailMate automatically.

## Installation

To install the bundle:

1. Go to the [Releases page](https://github.com/titancode/neovim-ghostty.mmbundle/releases) and download the `neovim-ghostty.mmBundle.zip` file.
2. Unzip the file to get a folder named `neovim-ghostty.mmBundle`.
3. Move the `neovim-ghostty.mmBundle` folder to the MailMate Bundles directory: `~/Library/Application Support/MailMate/Bundles/`.
4. Restart MailMate. You should now see a new option under the `Commands` menu: `Neovim (in Ghostty) > Edit`.

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

This bundle uses a shell script (`Support/bin/edit`) to launch Neovim inside Ghostty.  
The script locates the `nvim` executable using the `PATH` environment variable.

On macOS, GUI applications like MailMate do not normally inherit your full shell environment. To work around this, the script manually appends common binary paths to `PATH` at runtime:

- `/usr/local/bin` (Homebrew on Intel Macs)
- `/opt/homebrew/bin` (Homebrew on Apple Silicon Macs)
- `$HOME/bin` (for manual installs)

If the `brew` command is available, the script also adds `$(brew --prefix)/bin` to `PATH`.  
This ensures compatibility with non-default Homebrew installations that may use a custom prefix.

If `nvim` is installed in a different location that isn’t covered by these, the script will not find it and will exit with an error.

In that case, you can either:

- Add the appropriate path to your `PATH` environment variable, or
- Modify `Support/bin/edit` to manually set the `nvim` path

Also make sure Ghostty is installed and accessible via `/usr/bin/open -na "Ghostty"`.

### About Ghostty 1.2.0 and Later

Starting with [Ghostty 1.2.0](https://ghostty.org/docs/install/release-notes/1-2-0), commands launched through the `-e` flag are executed directly rather than being wrapped with `/bin/sh`. This update changes how command arguments are processed internally and slightly adjusts how terminal commands are invoked on macOS.

To accommodate this change, this bundle now uses a small, short-lived wrapper script created in the system’s temporary directory. The wrapper launches Neovim with the correct file path and deletes itself once Neovim exits.

This approach ensures that Neovim always launches reliably inside Ghostty on Ghostty 1.2.0 and later, while maintaining compatibility with earlier versions.