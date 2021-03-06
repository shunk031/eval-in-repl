**eval-in-repl: Consistent ESS-like eval interface for various REPLs**
--------------------

This package does what ESS does for R for various REPLs.

Emacs Speaks Statistics (ESS) package has a nice function called ess-eval-region-or-line-and-step, which is assigned to C-RET. This function sends a line or a selected region to the corresponding shell (R, Julia, Stata, etc) visibly. It also start up a shell if there is none.

This package implements similar work flow for various read-eval-print-loops (REPLs) shown below.

The languages currently supported are: **Emacs Lisp**, **Clojure**, **Common Lisp**, **Racket**, **Scheme**, **Hy**, **Python**, **Ruby**, **Standard ML**, **OCaml**, **Prolog**, and **shell script**.


**Usage: C-RET rules all**
--------------------

After installation and appropriate configuration (see below), you can use C-RET in a source file to start up an appropriate REPL and evaluate a line, selected region or the current expression depending on the context. The script will be shown on the right, and the REPL on the left. The REPL shows both the code executed and the value the code evaluated to. The cursor steps to the next expression in the source file (only when invoked without a selected region). A more detailed explanation is available at Qiita (http://qiita.com/kaz-yos/items/bb8016ec79cfbbf328df ).

**Emacs Lisp via IELM (screencast)**

You can see C-RET in action.

![ielm](screencast_ielm.gif?raw=true "ielm example")

YouTube vide: https://www.youtube.com/watch?v=gNBlF67e-0w&feature=youtu.be

**Clojure via cider.el**

![cider](screen_shot_cider.png?raw=true "cider example")

**Python via python.el**

![python](screen_shot_python.png?raw=true "python example")

**Shell script via essh.el**

![shell](screen_shot_shell.png?raw=true "shell example")


**Installation**
--------------------

eval-in-repl.el is available on the MELPA repository. You can do the following, then choose and install eval-in-repl.

```
M-x list-packages
```

To configure the MELPA, see this: http://melpa.milkbox.net/#/getting-started


The following files are included in the package. There are respective dependencies for each language-specific file that are NOT automatically installed.

- eval-in-repl.el
 - Skeleton package required by all specialized packages below.

- eval-in-repl-ielm.el (depends on IELM; part of default emacs installation)
 - Support for Inferior Emacs Lisp Mode

- eval-in-repl-cider.el (depends on cider.el)
 - Support for Clojure via cider.el

- eval-in-repl-slime.el (depends on slime.el)
 - Support for Common Lisp via slime.el

- eval-in-repl-geiser.el (depends on geiser.el)
 - Support for Racket and Guile Scheme via geiser.el

- eval-in-repl-racket.el (depends on racket-mode.el)
 - Support for Racket via racket-mode.el

- eval-in-repl-scheme.el
 - Support for Scheme via scheme.el (depends on scheme.el and cmuscheme.el; both part of default emacs installation)

- eval-in-repl-hy.el (depends on hy-mode.el)
 - Support for Hy via hy-mode.el


- eval-in-repl-python.el
 - Support for Python via python.el (depends on python.el; part of default emacs installation)

- eval-in-repl-ruby.el (depends on ruby-mode.el, inf-ruby.el, and ess.el)
 - Support for Ruby via ruby-mode.el

- eval-in-repl-sml.el (depends on sml-mode.el and ess.el)
 - Support for Standard ML via sml-mode.el

- eval-in-repl-ocaml.el (depends on tuareg.el and ess.el)
 - Support for OCaml via tuareg.el

- eval-in-repl-prolog.el (depends on prolog.el; part of default emacs installation)
 - Support for Prolog via prolog.el

- eval-in-repl-shell.el (depends on essh.el)
 - Support for shell via essh.el


**Configuration**
--------------------

The full configuration is the following. ```eval-in-repl.el``` is always necessary. Require other files as needed and configure the respective mode-specific key bindings.

```lisp
;; require the main file containing common functions
(require 'eval-in-repl)

;; Uncomment if no need to jump after evaluating current line
;; (setq eir-jump-after-eval nil)


;; ielm support (for emacs lisp)
(require 'eval-in-repl-ielm)
;; for .el files
(define-key emacs-lisp-mode-map (kbd "<C-return>") 'eir-eval-in-ielm)
;; for *scratch*
(define-key lisp-interaction-mode-map (kbd "<C-return>") 'eir-eval-in-ielm)
;; for M-x info
(define-key Info-mode-map (kbd "<C-return>") 'eir-eval-in-ielm)

;; cider support (for Clojure)
;; (require 'cider) ; if not done elsewhere
(require 'eval-in-repl-cider)
(define-key clojure-mode-map (kbd "<C-return>") 'eir-eval-in-cider)

;; SLIME support (for Common Lisp)
;; (require 'slime) ; if not done elsewhere
(require 'eval-in-repl-slime)
(add-hook 'lisp-mode-hook
		  '(lambda ()
		     (local-set-key (kbd "<C-return>") 'eir-eval-in-slime)))

;; Geiser support (for Racket and Guile Scheme)
;; When using this, turn off racket-mode and scheme supports
;; (require 'geiser) ; if not done elsewhere
(require 'eval-in-repl-geiser)
(add-hook 'geiser-mode-hook
		  '(lambda ()
		     (local-set-key (kbd "<C-return>") 'eir-eval-in-geiser)))

;; racket-mode support (for Racket; if not using Geiser)
;; (require 'racket-mode) ; if not done elsewhere
;; (require 'eval-in-repl-racket)
;; (define-key racket-mode-map (kbd "<C-return>") 'eir-eval-in-racket)

;; Scheme support (if not using Geiser))
;; (require 'scheme)    ; if not done elsewhere
;; (require 'cmuscheme) ; if not done elsewhere
;; (require 'eval-in-repl-scheme)
;; (add-hook 'scheme-mode-hook
;; 	  '(lambda ()
;; 	     (local-set-key (kbd "<C-return>") 'eir-eval-in-scheme)))

;; Hy support
;; (require 'hy-mode) ; if not done elsewhere
(require 'eval-in-repl-hy)
(define-key hy-mode-map (kbd "<C-return>") 'eir-eval-in-hy)


;; Python support
;; (require 'python) ; if not done elsewhere
(require 'eval-in-repl-python)
(define-key python-mode-map (kbd "<C-return>") 'eir-eval-in-python)

;; Ruby support
;; (require 'ruby-mode) ; if not done elsewhere
;; (require 'inf-ruby)  ; if not done elsewhere
;; (require 'ess)       ; if not done elsewhere
(require 'eval-in-repl-ruby)
(define-key ruby-mode-map (kbd "<C-return>") 'eir-eval-in-ruby)

;; SML support
;; (require 'sml-mode) ; if not done elsewhere
(require 'eval-in-repl-sml)
(define-key sml-mode-map (kbd "<C-return>") 'eir-eval-in-sml)
(define-key sml-mode-map (kbd "C-;") 'eir-send-to-sml-semicolon)

;; OCaml support
;; (require 'tuareg) ; if not done elsewhere
(require 'eval-in-repl-ocaml)
(define-key tuareg-mode-map (kbd "<C-return>") 'eir-eval-in-ocaml)
;; function to send a semicolon to OCaml REPL
(define-key tuareg-mode-map (kbd "C-;") 'eir-send-to-ocaml-semicolon)

;; Prolog support (Contributed by m00nlight)
;; if not done elsewhere
;; (autoload 'run-prolog "prolog" "Start a Prolog sub-process." t)
;; (autoload 'prolog-mode "prolog" "Major mode for editing Prolog programs." t)
;; (autoload 'mercury-mode "prolog" "Major mode for editing Mercury programs." t)
;; (setq prolog-system 'swi)
;; (setq auto-mode-alist (append '(("\\.pl$" . prolog-mode)
;;                                 ("\\.m$" . mercury-mode))
;;                                auto-mode-alist))
(require 'eval-in-repl-prolog)
(add-hook 'prolog-mode-hook
	  '(lambda ()
	     (local-set-key (kbd "<C-return>") 'eir-eval-in-prolog)))


;; Shell support
;; (require 'essh) ; if not done elsewhere
(require 'eval-in-repl-shell)
(add-hook 'sh-mode-hook
          '(lambda()
	     (local-set-key (kbd "C-<return>") 'eir-eval-in-shell)))
;; Version with opposite behavior to eir-jump-after-eval configuration
(defun eir-eval-in-shell2 ()
  "eval-in-repl for shell script (opposite behavior)

This version has the opposite behavior to the eir-jump-after-eval
configuration when invoked to evaluate a line."
  (interactive)
  (let ((eir-jump-after-eval (not eir-jump-after-eval)))
       (eir-eval-in-shell)))
(add-hook 'sh-mode-hook
          '(lambda()
	     (local-set-key (kbd "C-M-<return>") 'eir-eval-in-shell2)))

;; prolog support
;; if not done elsewhere
;; (autoload 'run-prolog "prolog" "Start a Prolog sub-process." t)
;; (autoload 'prolog-mode "prolog" "Major mode for editing Prolog programs." t)
;; (autoload 'mercury-mode "prolog" "Major mode for editing Mercury programs." t)
;; (setq prolog-system 'swi)
;; (setq auto-mode-alist (append '(("\\.pl$" . prolog-mode)
;;                                 ("\\.m$" . mercury-mode))
;;                                auto-mode-alist))
(require 'eval-in-repl-prolog)
(add-hook 'prolog-mode-hook
	  '(lambda ()
	     (local-set-key (kbd "<C-return>") 'eir-eval-in-prolog)))
```

**Known issues**
--------------------

- The first invocation of a cider REPL is slow and sometimes fails.
- If there is no \*cider-repl\*, but \*nrepl-...\* buffers, the latter are killed. This behavior may not be safe.
- The Geiser support is incompatible with the racket-mode support (racket-mode major mode is incompatible with Geiser) and with the scheme-mode support (Geiser will invoke Guile Scheme for .scm files).


**Version histoy**
--------------------

- 2015-09-05 0.7.0 Add Prolog support (Thanks m00nlight); no jump option for other languages
- 2015-06-05 0.6.0 Add defcustom configuration to configure whether to jump after eval (Thanks arichiardi)
- 2014-12-28 0.5.1 Refactoring, comment and documentation changes.
- 2014-12-21 0.5.0 Add Hy and OCaml support
- 2014-12-04 0.4.1 Require slime-repl.el (Thanks syohex)
- 2014-11-26 0.4.0 Add Ruby support
- 2014-11-12 0.3.0 Add Standard ML support
- 2014-09-13 0.2.1 Add EOF handling for Python
- 2014-08-30 0.2.0 Add Geiser and Racket support
- 2014-07-06 0.1.1 Delete excess autoload macros, add paredit.el to dependency
- 2014-06-30 0.1.0 First MELPA Release


**Special thanks:**
--------------------

- This package was inspired by the wonderful Emacs Speaks Statistics (ESS) package.
- David Högberg https://github.com/dfh contributed temporary reversal of no jump configuration.
- Yushi Wang https://github.com/m00nlight contributed the Prolog support.
- Andrea Richiardi https://github.com/arichiardi contributed the no jump customization.
- Syohei YOSHIDA https://github.com/syohex contributed fixing a missing dependency.
