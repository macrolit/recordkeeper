

<img width="2753" height="496" alt="Screenshot from 2026-02-22 09-10-17" src="https://github.com/user-attachments/assets/7bc25145-5776-4574-822b-df49b4f6142e" />

# Record Keeper

Advanced CLI tool for automation of Personal Knowledge Management

## What is it?

A modular, highly-configurable, platform-agnostic, extensible, universal file conversion workflow for bulk processing and decentralized archival of exportable collections from any source.

## What does it currently do?

The project is supposed to serve as a standardized playground for media conversion, retrieval, export and enrichment. Serving as a ground base structure and playground for utilization of conversion scripts along with LLM enhancement.

The initial scope is to:

- Turn your social media exports into .md notes with flexible properties.
- Integrate seamlessly with RRSS feeds and categorize your highlights
- Avoid Zettelkasten chaos and aglomeration
- File-system based categorization

It supports a flexible info-management workflow by converting individual entries from most filetype contents imports into yaml frontmatter (.md). 
The project also features optional remote or local (ollama) LLM categorization and enrichment.

<img width="3759" height="1944" alt="Screenshot from 2026-02-22 10-08-48" src="https://github.com/user-attachments/assets/9211cc5c-5ef4-4c1e-aa1f-5392285ebecb" /> (example of end result for a games library export)

## Is it for PKM's only? (Personal Knowledge Managers)

The project is not locked down to note-taking or archival centric workflows.

However, it initially ships with a strong intended focus for yaml-markdown use-cases. Support for other parsing mechanisms can be easily integrated, 
such as JSON metadata support, specific conversions for platform migration or for automation using tools like N8N.

Since of it's modular agnosticism, the same workflow can also be realistically utilized for the following use cases:

- Direct image-to-text bulk automation
- File-type generation
- Any combination of format-to-format file conversions
- Global multimedia interpretation and transcription engines
- Sortcut adaptations for live LLM completions for text editing
- Account/platform migration


>  "The missing link between website exports, knowledge management tools, categorization and 
>  AI enrichment - with a CLI-first, plugin-extensible, git-native architecture."

![2026-02-16 04-03-33](https://github.com/user-attachments/assets/c8b90378-2549-4401-95f4-976848a73a00)


[![Visual Demo](https://img.shields.io/badge/Watch%20Demo-GIF-blue?style=flat&logo=github)](https://i.postimg.cc/0j8pVF7t/edit24.gif)
 (Example of visual display)

## How to install?

Simply clone the git repo and install it with pipx and -e (editable) in an accessible location (you can move it to /bin or have it anywhere on disk)

```sh
git clone https://github.com/macrolit/recordkeeper
cd recordkeeper
pipx install -e . --python3.11
```
Currently stable using python <3.12

## How to use it?

Test the tool
`rk -h`

```sh
Notes:
  - Source and type names are case-insensitive
  - Initialization prevents overwriting existing directories
  - Structure is populated from the 'structure:' field in source config files
  - Multiple instances can exist at different locations; use 'cd' to navigate

# Examples:
  # Directory binding and navigation
  rk bind ~/Documents/collections     # bind directory so you don't have to specify paths every time
  rk ls @                             # Should output ~/Documents/collections , along with subdirs (bookmarks, etc.)
  rk ls                               # Should output ~/Documents/collections , along with subdirs (bookmarks, etc.)
  rk sel bookmarks                    # Select. Similar to "cd" command. Path resolution (pr) can jump down up to a depth of 2 dirs without sel
  rk <command> <args> @firefox        # Theoretical case where firefox exists in "bookmarks/firefox", no need to sel
  rk <command> <args> @firefox/user   # Out of pr reach, use either "rk ... @firefox/user" or "rk sel bookmars/firefox" then "rk ... @user" 
  rk unbind                           # Unbind ~/Documents/collections. Bound dirs history can be viewed with "rk init ls". You can use path                                         resolutions anywhere regardless of current pwd. 

# Run examples
  rk convert <args> /home/user/here /home/user/recordkeeper/tmp/                # Rely completely in the current script args for path handling
  rk convert <args> input=/home/user/here output=/home/user/recordkeeper/tmp/   # Use standardized path handling (same across scripts)
  rk convert <args> input /home/user/here output /home/user/recordkeeper/tmp/   # Same
  rk convert <args> in=/home/user/here out=/home/user/recordkeeper/tmp/         # Same
  rk convert <args> in /home/user/here out /home/user/recordkeeper/tmp/         # Same
  rk convert <args> in=@ out=%tmp/                                               # Same (recommended approach) using
                                                                                 # bound + rk path resolution


# Initialize sources and types
  rk init                           # List available sources
  rk init Books                     # List types for Books
  rk init Books physical            # Create Books/physical/ with structure
  rk init Books all                 # Create all Books types (empty)
  rk init Books full all            # Create all Books types with structures
  rk init Books physical none       # Create Books/ebook/ empty (no structure)
  
# Initialize everything
  rk init full                      # Create all source folders
  rk init all                       # Create all sources + types (empty)
  rk init full all                  # Create everything with full structure
  
# View tracking
  rk init list                      # Show all tracked directories
  rk init ls                        # Alias to "rk init list"
  rk init list config               # Show available config files

# Data conversion and processing
  rk convert obsidian tmp/file.md tmp/output.yaml                                 # use a conversion community plugin "obsidian" (obsidian.py
                                                                                  # stored in src/modules/conversion/obsidian.py)

  rk convert obsidian -q -f tmp/file.md tmp/output.yaml                           # works assuming you are located in recordkeeper folder btw

  rk convert using direct bookmarks in=tmp/input.html out=tmp/output.yaml         # use a conversion chain (using)
                                                                                  # "direct.py" piped into "bookmarks.py" both stored in
                                                                                  # "src/modules/conversion/"

  rk retrieve bookmark_fetcher in=%tmp/bookmarks.yaml out=%tmp/r-bookmarks.yaml   # runs plugin bookmark_fetcher.py from
                                                                                  # "src/modules/retrieval/"

  rk automate <platform> -s in=%tmp/storage/ out=@sorted/                         # Uses LLM automation with platform-specific prompts
                                                                                  # defined in src/rk/prompts, overriding config/service.yaml
                                                                                  # (your config) with the -s flag (--serverless-inference)
                                                                                  # forcing remote inference

  rk automate <platform> in=%tmp/stored/ out=@categories/                         # Uses AI-model bulk automation with platform-specific 
                                                                                  # prompts defined in src/rk/prompts, using your config
                                                                                  # (config/service.yaml) . It will modify frontmatter 
                                                                                  # (e.g. "category:") from input files, it also appends 
                                                                                  # markdown text in the file as defined in the 
                                                                                  # "src/rk/prompts" it will make in-place editing 
                                                                                  # to files located in the input directory.
                                                                                  # Output dir structure will be included for prompt reference 
                                                                                  # and MUST have been structured with category folders,
                                                                                  # either by creating them manually or populate using 
                                                                                  # "rk init <source> <type>". Otherwise the prompt WON'T INCLUDE
                                                                                  # ANY CATEGORIES and you will potentially waste credits.
                                                                                  # (sources and types can be configured as well, they are simple 
                                                                                  # .yaml files at "config/sources/". you can simply create folders 
                                                                                  # manually too)

  rk parse using yaml in=tmp/data.yaml out=tmp/clean.yaml                         # Uses a script chain for parsing YAML stored in src/rk/parsing/yaml/ .
                                                                                  # Currently there is only a YAML parser (more parser types in the
                                                                                  # future as needed)

# Organize categorized files into folders
  rk arrange 
  rk arrange settle <input_directory> <output_root_directory>                     # Uses "category" frontmatter field for copying .md files
                                                                                  # from input dir into relevant category folders at destination.
                                                                                  # Output directory MUST contain the relevant folder structure
                                                                                  # (create with "rk init <source> <type>") or else it will copy 
                                                                                  # all as "uncategorized"
# Module manager (github plugins)
  rk mod list                          # list all modules
  rk mod list core                     # list stored in "src/rk/" 
  rk mod list external                 # list stored in "src/modules/" 
  rk mod get <username/gitrepo>
  rk mod remove <username/gitrepo>
  rk mod info <name>
  
```


![Note Formatting](https://i.postimg.cc/mgd6CNWx/Screenshot-from-2026-02-16-05-26-02.png) (Example of categorized and enriched note)

## Features

- Convert and normalize exports into Markdown with YAML frontmatter.
- Bulk-process large archives or collections from any source.
- Enrich and categorize files with local or remote LLM models.
- Integrate cleanly with markdown-based PKM systems.
- Automate processing with a flexible user-driven plugin/module system.

## Module System

recordkeeper supports git-managed plugin modules for parsing, processing, automation, and prompts.
Anyone can easily contribute by simply uploading an installable plugin to github.


## Open Source Models 

==The tool will ship along a dedicated endpoint==
An AI categorization endpoint will be launched along the tool, so that users can quickly try it out!
Tuned infrastructure for large cost-efficient workloads, easy to use and try out with curl commands

It's always opt-in, optional and configurable. You can simply use ollama or your preference of infrastructure

(support for other OpenAi-compatible generic localhost api's soon!)


### Markdown Accent

The project currently supports standard GFM (Git Flavored Markdown) with YAML frontmatter as metadata.
Support for other metadata types may be added in future updates.


### Individual Online Site Support

The tool will ship with general basic support for `JSON , CSV , HTML,` and etc. 
As well as specific conversion handlers for data export formats such as Google-Takeout, Meta Exports, Bookmarks and more.
For new conversion methods, handlers or platforms not yet supported; 
                                          ==Any individual is free to create a dedicated plugin or submit a PR to be added as core utility.==
Documentation is provided as well as guides for creating modules.

### Expand beyond basics

The `rk init <source> <type>` ships an array of category directories. 
You can easily create or remove categories and subcategories manually and these will be valid sources for directory creation as `<source>`.
The prompts can be edited in the `src/rk/prompts/` and directories will be recognized as categories.
You can also create new .yaml files in the `config/sources/` directory or modify existing, these will be added as category context for the LLM prompt.
For personalized LLM processing, you can define custom system messages as well.


![Note Formatting](https://i.postimg.cc/WpXr29G4/Screenshot-from-2026-02-16-04-26-57.png) (showcase of automated context retrieval)


# Self managed, decentraliced activity management

## Keep your footprints

Secure and backup your valued information

Properly track and archive data throughout account migrations

Import and store relevant activity from your online profiles

Essential tool for giving users control over their records

## Browse with Relief

Protect against user policies embracing data loss or account suspension

Avoid reliance on third-party servers that don't care about your data

Backup valuable information stored on infrastructure you don't fully know or trust

## Internet Hygiene

Address, prevent and minimize unnecessary rss recurrence

Teach and rewire platform algorythms about what actually matters

Formalize your activity with curated resources

Cut ties with undesired online behavior

## AI the right way

Filter out noise from your collections

Automate connections in your notes

Elevate RRSS feeds to the next level


[![Visual Portrait Demo](https://img.shields.io/badge/Watch%20Demo-GIF-blue?style=flat&logo=github)](https://i.postimg.cc/zfbphsk2/edit14.gif) (Visual Portrait Demo)


# RecordKeeper's Unique Position

## Similar tools exist, but none combine these features:

**vs. bookmark managers (buku, linkding, shiori):**
- Portable YAML files (not locked in a database)
- Plugin system for extensibility
- AI-powered metadata enrichment

**vs. web clippers (Obsidian clipper, Notion clipper):**
- Works with bulk HTML exports (not one-at-a-time)
- Imports from most-if-not-all file types instead from HTML only
- Editor-agnostic (works with Obsidian, Emacs, or plain files)
- Self-contained, deployable (no browser extension required)

**vs. archival tools (ArchiveBox, Wallabag):**
- Lightweight metadata focus (not full page archival)
- CLI-first (not web UI)
- Personal Knowledge management integration
- AI enhancements and automated categorisation

**vs. data liberation tools (yt-dlp, gallery-dl):**
- Easy integration with existing repositories
- Parsing, organization and enrichment only (not extraction)
- Multi-platform unified handling



>  `rk` is the tool for when platforms lack export APIs and you need
>  your saved content in a format that will outlive social media hosting.


# No conversion support for your export type? make your own!

Plugins are very easy to make, 
- Use existing templates 
- Simply follow a guideline
- Vibe-coding ready prompts. just copy-paste
- Package manager `mod` as using `rk mod get username/plugin` with smart git integration

Plugins are dead-simple to make. Please read the PLUGINS.md and simply follow the guide.


Share your plugin in [website still not decided] and help the community grow!


# No export? No problem

IF you can acces YOUR data through a browser, it's completely yours.
Use dev tools and parse HTML with two clicks. No built-in export required.

# For developers

You can review the `cli.py` logic and make adjustments to your preferences or add feature improvements.
Code changes are instant and live, so you don't need to reload or recompile anything.

Make a pull request and will be reviewed ASAP

## File Processing Workflow

Default: Export > Convert > Parse > Process > Deploy 

Optional: Retrieve, Categorize(AI), Enrich(AI)




# Editor Support Beyond Basic Markdown

Any Markdown-enabled editor should be sufficient for viewing and editing the generated notes.

Nevertheless, some PKMs have more comprehensive toolkits and feature support than others. 
So here is a feature breakdown for the most suitable popular Personal Knowledge Managers; 

| PKM                  | YAML Parsing                         | Image Embeds (HTTP)    | Wiki Links                     | Querying                               | Notes Database                        | Large Vault (>5K) | MD (GFM) Exceptions                  | Notes Display                                                                     |
| -------------------- | ------------------------------------ | ---------------------- | ------------------------------ | -------------------------------------- | ------------------------------------- | ----------------- | ------------------------------------ | --------------------------------------------------------------------------------- |
| **Emacs**            | ✅ Full support                       | ✅ Native GUI images    | ✅ Via plugins                  | ✅ Org-roam/queries                     | ✅ Org-roam DB                         | ✅ Excellent       | ✅ Org-mode to Markdown adaptation    | Live Org-mode to MD rendering                                                     |
| **Obsidian (Bases)** | ✅ Properties UI parsed               | ✅ MD Live              | ✅ `[[ ]]` native               | ✅ Bases(native) or Dataview (SQL-like) | ✅ Bases (views/tables) or Dataview DB | ✅ Excellent       | ✅ Supports OFM + GFM                 | Live MD rendering                                                                 |
| **TiddlyWiki**       | ✅ Importing or Via plugins           | ✅ Using HTML img src   | ✅ `[[ ]]`                      | ✅ Powerful filters/macros              | ✅ Filter language/macros              | ✅ Excellent       | ✅ Converted to Wikitext syntax       | Live WikiText, MD for reading view using plugins                                  |
| **SiYuan**           | ✅ Import/export                      | ✅ Block embeds         | ✅ `[[ ]]` (imported into json) | ✅ Block queries                        | ✅ Block-based DB                      | ✅ Good            | ✅ All MD converted into Block syntax | Live block editor. No MD                                                          |
| **SilverBullet**     | ✅ Index queries                      | ✅ MD Live preview      | ✅ `[[ ]]`                      | ✅ `^` queries native                   | ✅ Full-text index                     | ✅ Excellent       | ✅ None                               | Live MD rendering per-line                                                        |
| **Neovim**           | ✅ LSP parsing, obsidian.nvim support | ✅ Via Plugins          | ✅ Via plugins                  | ✅ Telescope queries                    | ✅ Neorg workspace                     | ✅ Excellent       | ✅ Neorg syntax adaptation            | Live LSP<br>//<br>Browser sync <br>//<br>Live TUI  emulator overlay using plugins |
| **VSCode**           | ✅ Front-matter extension             | ✅ Preview Via Plugins  | ✅ Foam `[[ ]]`                 | ✅ Dendron queries                      | ✅ Dendron schema                      | ✅ Good            | ✅ None                               | No inline rendering. Preview MD via command                                       |
| **QOwnNotes**        | ✅ Tags/metadata                      | ✅ MD Live preview      | ⚠️ Custom links                | 🟡 Tag search only                     | ❌ Folder-based                        | ✅ Great           | ✅ None                               | Live preview pane                                                                 |
| **Logseq**           | ❌ Block properties only              | ✅ Block embeds         | ✅ `[[ ]]` blocks               | ✅ Live queries                         | ✅ Block DB                            | ⚠️ Slow           | ✅ GFM + Syntax Variations            | Live syntax highlight.<br>Requires switching for preview                          |
| **Zettlr**           | ⚠️ Preserves text only               | ✅ MD Live preview      | ❌ Pandoc links                 | ❌ Basic search                         | ❌ Project mgmt                        | ✅ Great           | ✅ GFM + Pandoc extensions            | Live preview pane                                                                 |
| **Trilium**          | ❌ Attributes only                    | ❌ Local images Only    | ✅ `[[ ]]`                      | ✅ JS relation queries                  | ✅ Full relation DB                    | ✅ Good            | ✅ GFM + HTML variations              | Live MD rendering                                                                 |
| **Sublime Text**     | ✅ YAML package                       | ✅ Plugins for previews | ❌ Plain text                   | ❌ None                                 | ❌ Project folders                     | ✅ Great           | ✅ Indifferent                        | Raw source mode. Plugins for previews                                             |
| **Typora**           | ⚠️ Preserves/reads                   | ✅ Live MD              | ❌ Plain text                   | ❌ None                                 | ❌ Folders only                        | ✅ Good            | ✅ Yes GFM + extensions               | Live MD rendering                                                                 |
| **MarkText**         | ⚠️ Preserves text only               | ✅ MD Live preview      | ❌ Plain text                   | ❌ None                                 | ❌ Folders                             | ✅ Great           | ✅ None                               | Live preview pane                                                                 |
| **Zim**              | ⚠️ Preserves text only               | ❌ No preview           | ❌ Wiki                         | ❌ Basic text search                    | ❌ Folder of .txt files                | ✅ Great           | ❌ Wiki. Lacking import capability    | Raw source mode. No MD                                                            |



## Advanced functionality for graphical setups
For users wanting more elaborate metadata utilization, such as Visual Library / Query Table Previews (like shown in the demo) here is a detailed breakdown; 

| PKM                  | Display Method                    | Metadata support for Queries                                                             | Lists + Thumbnails Linking                                                                          | Implementation Notes                                                                                                                  |
| -------------------- | --------------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Obsidian (Bases)** | ✅ Local image links or URL embeds | ✅ Native                                                                             | ✅ Full out-of-the-box(title/tags → Bases views)                                                     | Bases queries `title: "X" AND tags: "#work"` → thumbnail grid                                                                         |
| **Emacs GUI**        | ✅ `image-dired` + packages        | ✅ Native                                                                             | ✅ org-roam + dired thumbnails + query                                                               | `consult-org-roam` + `image-dired` → YAML tags → image gallery                                                                        |
| **TiddlyWiki**           | ✅ HTML tags for URL embeds        | ✅ Imports to native fields, else YAML<br>plugins parse to tiddler fields.            | ✅ Via filters, gallery plugins show thumbnails from covers                                          | **Filter** queries tiddlers → Template renders each as card → CSS Grid/Flexbox: done.<br>(Alternatively just use **Gallery plugins**) |
| **SiYuan**           | ✅ Image Block Embeds              | 🟡 Import/export workflow → Internal attributes                                      | ✅ Queries → Block properties → cards                                                                | Cards (formerly gallery) shows block content and images                                                                               |
| **Neovim+Kitty**     | ✅ Plugins + Kitty protocol setup | ✅ obsidian.nvim or other dedicated parsing                                          | 🟡 Rich telescope previews + yaml frontmatter parsers + queried tables + managing Kitty constraints | Technically possible but demanding (plugin combinations may achieve this)                                                             |
| **Logseq**           | ✅ Local + URL Image Embeds        | ⚠️ Only block properties supported (manual conversion)                               | ✅ Property queries → custom CSS view → Card Queries                                                 | `{{query (property status in-progress)}}` → CSS panel → visual blocks/lists                                                           |
| **Trilium**          | ✅ Local image links               | ⚠️ Attributes format only (manual conversion needed)                                 | ✅ Built‑in list → thumbnail‑style via extension                                                     | Tree shows content → YAML to attributes conversion →  thumbnail‑style via extension "Collection Views"                                |
| **QOwnNotes**        | ✅ Local image links               | ✅ YAML → Tags/metadata                                                               | ❌ Tag search → list → workarounds → local image file lists only                                     | Local thumbnails via file manager + tag search integration. No true visual display or queries.                                        |
| **VSCode+Dendron**   | ✅ Dendron images                  | 🟡 Front-matter extension → internal metadata                                        | ❌ No current Dendron integration                                                                    | Requires creating a custom blocked querying plugin +<br>Another Dendron plugin integration for preview buffers                        |
| **SilverBullet**     | ✅ Local + URL Image Embeds        | ✅ Fully Natively                                                                     | ❌ Index queries + Text list, no thumbnails                                                          | `list from #work where status = "done"`                                                                                               |
| **Zettlr**           | ✅ Local image files or URL embeds | ✅ Native Yaml frontmatter handling → Pandoc metadata                                 | ❌ File list filtering, no thumbnails                                                                | Project manager only shows YAML-tagged files                                                                                          |
| **Sublime Text**     | ✅ With plugins + config           | ✅ YAML package                                                                       | ❌ Basic find                                                                                        | Sidebar only                                                                                                                          |
| **MarkText**         | ✅ Local or URL embeds             | ⚠️ Only preserves text                                                               | ❌ Simple text search                                                                                | File explorer only                                                                                                                    |
| **Typora**           | ✅ Local or URL embeds             | ⚠️ Only preserves text                                                               | ❌ Metadata filtering only                                                                           | File list only                                                                                                                        |
| **Zim**                  | ✅ Yes, displayed in preview pane  | ❌ No editor parsing/import. Native metadata: page properties in `key:: value` format | ❌ Basic text search                                                                                 | File list only                                                                                                                        |



## Future implementations

Take a look at the ==ROADMAP.md== for info


# Release Date

Anytime Q1 2026



# Support the release of the project through donations!




