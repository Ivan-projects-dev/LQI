Read [[General guide]] first

As `#` symbol cannot be added in note naming, [[Bernstein-Vazirani (Q)]] $=$ [[Bernstein-Vazirani]] in `Q#`

Plugins below may have functionality that was never used by author, so it will not be included here. In order to manage plugins, open: *Settings* $->$ *Community plugins*
![[Pasted image 20260429171834.png]]
### asciimath
Write math in AsciiMath syntax as alternative to LaTeX. Simpler to type for basic expressions
```asciimath
x = (-b +- sqrt(b^2 - 4ac)) / (2a)
```
LaTeX may be heavy for simple fractions, square roots, or basic algebra. For complex quantum notation (bra-ket, [[Tensor]] products), LaTeX is clearer.

---
### No More Flickering Inline Math (`inline-math`)
Eliminates the visual flicker that occurs in Obsidian when the cursor moves in & out of inline math expressions like `$\psi$`.

---
### Obsidian Matrix (`obsidian-matrix`)
GUI helper for typing LaTeX [[Matrix]] notation. Opens grid editor so you can fill in cells & it generates the LaTeX code. Saves time counting `&` separators in multi-column matrices.

*Command Palette* (`Ctrl+P`) $→$ *[[Matrix]]: Insert [[Matrix]]* - then fill in dimensions & values. It is also possible to select different style for [[Matrix]]
![[Pasted image 20260429165800.png]]
![[Pasted image 20260429165735.png]]

---
### Note Linker (`obisidian-note-linker`)
**Purpose:** Scans the open note for text that matches existing note titles & suggests inserting `[[WikiLinks]]` automatically. Finds link opportunities you would otherwise miss.

Open note $→$ `Ctrl+Q` $→$ review suggestions $→$ accept or skip each one. It is possible to scan for notes math on either current note or on whole vault.
![[Pasted image 20260429170347.png]]
The image below demonstrates keyword ambiguity when $2$ different candidates from
![[Pasted image 20260429170416.png]]

---
### Strip Internal Links (`copy-without-links`)
Removes `[[WikiLink]]` syntax from text, leaving only the plain display text. $2$ modes: **copy to clipboard** (non-destructive) or **strip in-place** (modifies the file).
- `Ctrl+M` - immediately removes internal links in current file.
---
### Link Remover (`hyperlink-remover`)
**Purpose:** Removes links from the current file. Three targeted operations: all links, internal `[[WikiLinks]]` only, or external `[text](url)` only.
- `Ctrl+~`  - immediately remove all links (both internal & external) from the file (used by author)

---
### Global Search & Replace (`global-search-&-replace`)
Find & replace text across all files in the vault simultaneously. Supports regex. Plugin does not differ uppercase & lowercase letters, but it **does recognize empty space**!
`Mod+Y` $→$ enter search string $→$ enter replacement $→$ confirm.
![[Pasted image 20260429170651.png]]

---
### Image Captions (`image-captions`)
Renders alt-text as visible caption below images in preview mode. Standard Obsidian images show alt-text only on hover; this makes it visible inline. Writes images as `![[image.png|Caption text here]]` & the caption appears below the image in reading view.

All used images are stored in `z_img` folder. Image files have random names, as both `obisidian-note-linker` & `global-search-&-replace` take keywords

---
### Find Orphaned Images (`find-orphaned-images`)
Scans the vault for image files (PNG, JPG, SVG, etc.) that are not referenced in any note. Lists them so you can review & delete.

`Ctrl+R` - shows menu below & allows to delete all images that are stored in `z_img` but do not used by any node ("orphan images"). This menu has $2$ more options not used by the author
![[Pasted image 20260429171052.png]]
**Usage:** `Mod+R` $→$ a list of orphaned images appears. Delete individually or in bulk.
![[Pasted image 20260429171052.png]]

---
### Find Orphaned Files & Broken Links (`find-unlinked-files`)
**Purpose:** Two funcs - find notes with no backlinks (orphaned notes that nothing links to) & find unresolved links (WikiLinks that point to notes that do not exist).
- `Ctrl+Z` $→$ finds all unresolved `[[links]]` - links pointing to nonexistent notes by redirecting to [[broken links output]]
![[Pasted image 20260429165916.png]]




