# speed-type [![melpa badge][melpa-badge]][melpa-link] [![melpa stable badge][melpa-stable-badge]][melpa-stable-link]

Practice touch/speed typing in GNU Emacs.

![Screenshot](https://raw.github.com/dakra/speed-type/master/speed-type-screen-shot.png)

## Installation

Install speed-type from [MELPA](melpa.org) with:

```
M-x package-install RET speed-type
```

If you prefer to install by hand: Put speed-type.el into a directory specified
by the load-path variable. Alternatively, you can add a directory to the
variable load-path by (add-to-list 'load-path "ADDITIONAL-DIRECTORY").

If you put the file in "~/.emacs.d/speed-type/speed-type.el" for instance, the
following snipped in your .emacs file will load and init the extension.

```emacs-lisp
(add-to-list 'load-path "~/.emacs.d/speed-type/speed-type.el")
(require 'speed-type)
```

## Configuration

speed-type can be customized using:

```
M-x customize-group speed-type RET
```

you may also want to increase the text scale in `speed-type-mode`:
```
(add-hook 'speed-type-mode-hook (lambda () (text-scale-increase 3)))
```

## Running speed-type

Executing `M-x speed-type-text` will start the typing exercise. A new buffer will
open and a random text sample will appear. As you type the text it will change
color to show progress and highlight correct and incorrect entries. Timing
happens automatically, the clock starts on the first character typed and ends
with the last. Statistics like characters typed, words-per-minute, and total
time will be shown as soon as the last character is entered.

You can use any buffer or part of it to run speed-type. `M-x speed-type-region`
and `M-x speed-type-buffer` will do the same thing as speed-type-text, except they
take the text sample you've picked.
This works for programming code buffers/regions as well.

`speed-type-buffer` by default will only take a random portion of the buffer - If
you want the whole buffer, use `C-u speed-type-buffer`.

Random samples are taken from Project Gutenberg. A small number of books will be
downloaded on demand and stored in "~/emacs.d/speed-type". They will only be
downloaded once.

`speed-type-region` will start a speed-type session with the text from
the selected region.

`speed-type-top-x` (or -100/-1000) lets you practice the top X words
for the selected language.

### Reading PDFs using speed-type
File must first be converted to text and processed to remove unwanted spaces and characters:
```
(defun speedtype-open-pdf-txt (arg)
  "Open PDF file ARG as a txt file and clean text for speed-type, txt is stored in same dir."
    (interactive "fpdf: ")
    (shell-command
     (format (concat "pdftotext " (replace-regexp-in-string " " "?\ " arg) " -layout")))
    (let ((result (replace-regexp-in-string "pdf" "txt" arg)))
      (f-write-text (replace-regexp-in-string "" "" (with-temp-buffer (insert-file-contents result) (buffer-string))) 'utf-8 result);remove ^L control char
      (f-write-text (remove-excess-spaces (with-temp-buffer (insert-file-contents result) (buffer-string))) 'utf-8 result);remove excess spaces
      (f-write-text (remove-empty-lines   (with-temp-buffer (insert-file-contents result) (buffer-string))) 'utf-8 result)
      (find-file result)))
```
use `speed-type-region` on section of text, [n] key will move to next section.

[melpa-link]: https://melpa.org/#/speed-type
[melpa-stable-link]: https://stable.melpa.org/#/speed-type
[melpa-badge]: https://melpa.org/packages/speed-type-badge.svg
[melpa-stable-badge]: https://stable.melpa.org/packages/speed-type-badge.svg
