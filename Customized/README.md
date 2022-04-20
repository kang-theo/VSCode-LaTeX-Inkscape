# Two steps to make it work

> From [Gilles Castel: How I draw figures for my mathematical lecture notes using Inkscape](https://castel.dev/post/lecture-notes-2/):  If you want to try this out for yourself, you can find both my [script for managing figures in Vim](https://github.com/gillescastel/inkscape-figures) and my [Inkscape shortcut manager](https://github.com/gillescastel/inkscape-shortcut-manager)
> on Github. The first script should work out of the box on both Mac and Linux-based systems; the second only works on Linux (it uses Xlib) and is a bit harder to set up.

However, this only works on Linux+Vim, I find a Repo transfering the flow from Linux to macOS: [sleepymalc/VSCode-LaTeX-Inkscape](https://github.com/sleepymalc/VSCode-LaTeX-Inkscape) `<span id="context_snippet">` using "Context".

# Step One: inscape-figures

**Done**.

## Usage

1. Define a in notes project folder, modify the project path to the notes folder in setting.json. It must have a folder called "Figures". Keys below must be invoked when cursor is in a note tex file.
2. ``ctrl+i w``: Invoke inkscape-figures watch(fswatch is really used) to watch the notes project folder. If .svg is generated without pdf and pdf_tex, then generate them.
3. ``ctrl+i c``: Automatically create inkscape document and insert the figures in a note tex file.
   It does:
   1) Copy the content under cursor (normally the figure name) into your clipboard
   2) Delete that copied content and insert a snippet defined in latex.json (defined in vscode->user.snippets->latex.code-snippets).
   3) Lastly, send a command in a terminal by command runner, with the command inkscapeCreate(defined in vscode setting.json). This command will open a new document with name specified in 1) and insert snippet for figures in tex file.
   4) Draw pictures in inkscape and save the project(command+s), and rename the caption in the note tex file.
   5) Rebuild tex and check the pdf updated to see the effect.
4. ``ctrl+i e``: Open the figures (with ''choose'') again to modified them as needed later.

# Step Two: inkscape shortcut manager

**Todo**.

From “[sleepymalc](https://github.com/sleepymalc)/[VSCode-LaTeX-Inkscape](https://github.com/sleepymalc/VSCode-LaTeX-Inkscape)”: After some research, although there is a way to let the original script in [inkscape-shortcut-manager](https://github.com/gillescastel/inkscape-shortcut-manager) run correctly since it depends on **xlib**, which is no longer used by macOS for almost every application(including Inkscape, as expected). Hence, the only thing I can do now is to give up. In a perceivable future, if I have time to find an alternative way to interrupt the window activity in macOS, I'll try to configure it for macOS.

Stay tuned.

# Misc
## 1. Problem of snippet (To be resolved)
Try to get familiar with HyperSnippets extension to solve the auto completion in non mathetical environment.

> One thing to consider when writing these snippets is, ‘will these snippets collide with usual text?’ For example, according to my dictionary, there are about 72 words in English and 2000 words in Dutch that contain  **`sr`**, which means that while I’m typing the word **`disregard`**, the **`sr`** would expand to **`^2`**, giving me **`di^2egard`**.
>
> The solution to this problem is adding a **context** to snippets. Using the syntax highlighting of Vim, it can be determined whether or not UltiSnips should expand the snippet depending if you’re in math or text. Add the following to the top of your snippets file:
> [How I&#39;m able to take notes in mathematics lectures using LaTeX and Vim](https://castel.dev/post/lecture-notes-1/#context) [#Context](#context_snippet)
## 2. Excalidraw.com integration
   [Excalidraw.com](https://excalidraw.com) can draw and export **.svg** figures, so:

   1. Draw figures in Excalidraw, and export as **svg**.
   2. Copy figures into kTestNote/Figures. The ```inscape-figures watch``` will monitor the figures and invoke Inkscape to generate **pdf_tex** and **pdf**.
   3. Use ```ctrl+i e``` to invoke ```Inscape-figures edit``` to edit the figures in Inkscape(like adding some text, mathmatic formula, etc.), taking the advantage of its pdf_tex pros.
   4. Add code below to kTestNote.tex and build the it.
      ```latex
      \begin{figure}[H]
         \centering
         \incfig{font_test_of_excalidraw_svg_in_inkscape}
         \caption{Reedit Excalidraw svg in Inkscape}
         \label{fig:font_test_of_excalidraw_svg_in_inkscape}
      \end{figure}
      ```
## 3. Excel tables integration
Like Inkscpae, we can add some support for tables.
> Automation work still need to be done.

Create tables generated from Excel using [Excel2LaTeX](https://github.com/ivankokan/Excel2LaTeX) add-in:
1. The tables can be drawn with hand in Excel: ```Home->Draw outside border->Draw border```.
2. Export LaTeX code by **Excel2LaTeX**.
3. Paste in kTestNote.tex, using packages: ```booktabs and multirow```.
4. Manually add diagbox needed, since Excel2LaTeC cannot export slant line, using package ```diagbox```

ps.
1. Typora can also export Excel tables, however, unsatisfactory.
2. We can also draw tables using: [LaTeX-Tables](https://www.latex-tables.com) and [Tables-Generator](https://www.tablesgenerator.com/latex_tables).

## 4. For whom not familiar with commands for math formula
```Mathpix``` ```EquationMaker``` ```Detexify``` ```Mathkey``` ```Mathcha``` ```Quiver```

<img src="https://mathpix.com/images/logo/image-logo.png" width="48">
<img src="https://static.macupdate.com/products/50374/m/equation-maker-logo.png?v=1574176973" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRt9b_RvPnFyzuoFPaycQGR46ciRmi11r1FEQ&usqp=CAU" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHtkytJ85bF9V6v2lTpoXoqcI8JVjlW2KLhLrHgeCEma6uKsyo_aOnNxaczNr5Zz6CPdo&usqp=CAU" width="48">
<img src="https://www.mathcha.io/image/notebook-icon.png" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ5rMHnsUB1YwfZkaSaac7E75_xsjqGK0BYFrLy0XHf3etrOTgGxgBbdHHU7fkoL2zIz0I&usqp=CAU" width="48">
