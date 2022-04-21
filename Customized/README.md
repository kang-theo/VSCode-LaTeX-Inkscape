Table of Contents
=================

- [Table of Contents](#table-of-contents)
- [Two steps to make it work](#two-steps-to-make-it-work)
  - [Step One: inscape-figures](#step-one-inscape-figures)
    - [Usage of "vscode + inkscape-figures"](#usage-of-vscode--inkscape-figures)
  - [Step Two: inkscape shortcut manager](#step-two-inkscape-shortcut-manager)
- [Misc](#misc)
  - [1. HyperSnips for math snippets](#1-hypersnips-for-math-snippets)
  - [2. Excalidraw.com integration](#2-excalidrawcom-integration)
  - [3. Excel tables integration](#3-excel-tables-integration)
  - [4. XMind mindmap integration](#4-xmind-mindmap-integration)
  - [5. For whom not familiar with latex commands for math formula](#5-for-whom-not-familiar-with-latex-commands-for-math-formula)
  - [6. Correcting spelling mistakes on the fly](#6-correcting-spelling-mistakes-on-the-fly)
- [Credits](#credits)
- [Related Projects](#related-projects)

# Two steps to make it work

> From [Gilles Castel: How I draw figures for my mathematical lecture notes using Inkscape](https://castel.dev/post/lecture-notes-2/):  If you want to try this out for yourself, you can find both my [script for managing figures in Vim](https://github.com/gillescastel/inkscape-figures) and my [Inkscape shortcut manager](https://github.com/gillescastel/inkscape-shortcut-manager)
> on Github. The first script should work out of the box on both Mac and Linux-based systems; the second only works on Linux (it uses Xlib) and is a bit harder to set up.

However, this only works on Linux+Vim, I find a Repo transfering the flow from Linux to macOS: [sleepymalc/VSCode-LaTeX-Inkscape](https://github.com/sleepymalc/VSCode-LaTeX-Inkscape).

## Step One: inscape-figures

- [x] **Done**.

### Usage of "vscode + inkscape-figures"

1. Specify a note project folder, modify the project path to the note folder in *setting.json*. It should have a folder called "*Figures*". Keys in steps below will be triggered when the cursor is in a tex file.
2. ```ctrl+i w```: Trigger ```inkscape-figures watch``` (*fswatch* is actually used) to watch the note project folder, in case that **.svg** is generated without **.pdf** and **.pdf_tex**. This will  monitor the folder and generate them.
3. ```ctrl+i c```: Automatically create inkscape document and insert the figures in a note tex file.
   It does:
   1) Copy the content under cursor (normally the figure name) into your clipboard.
   2) Delete that copied content and insert a snippet defined in latex.json (defined in *vscode->user.snippets->latex.code-snippets*).
   3) Lastly, send a command in a terminal by "*command runner*", with the command ```inkscapeCreate``` (defined in *vscode setting.json*). This command will open a new document with name specified in 1) and insert snippet for figures in the tex file.
   4) Draw pictures in inkscape and save the project (*command+s*), and rename the caption in the note tex file.
   5) Rebuild tex and check the pdf updated to see the effect.
4. ```ctrl+i e```: Open the figures (with "*choose*") again to modified them as needed later, corresponding to ```inkscapeEdit```.
5. ```ctrl+i s```: Stop the ``inkscape-figures watch``.

## Step Two: inkscape shortcut manager

- [ ] **Todo**.

From “[sleepymalc](https://github.com/sleepymalc)/[VSCode-LaTeX-Inkscape](https://github.com/sleepymalc/VSCode-LaTeX-Inkscape)”: After some research, although there is a way to let the original script in [inkscape-shortcut-manager](https://github.com/gillescastel/inkscape-shortcut-manager) run correctly since it depends on **xlib**, which is no longer used by macOS for almost every application(including Inkscape, as expected). Hence, the only thing I can do now is to give up. In a perceivable future, if I have time to find an alternative way to interrupt the window activity in macOS, I'll try to configure it for macOS.

Stay tuned.

# Misc
## 1. HyperSnips for math snippets

Use "context" in HyperSnips extension to solve the auto completion in non-mathetical environment.

> One thing to consider when writing these snippets is, ‘will these snippets collide with usual text?’ For example, according to my dictionary, there are about 72 words in English and 2000 words in Dutch that contain  **`sr`**, which means that while I’m typing the word **`disregard`**, the **`sr`** would expand to **`^2`**, giving me **`di^2egard`**.
>
> The solution to this problem is adding a **context** to snippets. Using the syntax highlighting of Vim, it can be determined whether or not UltiSnips should expand the snippet depending if you’re in math or text. Add the following to the top of your snippets file:
> [How I&#39;m able to take notes in mathematics lectures using LaTeX and Vim](https://castel.dev/post/lecture-notes-1/#context)#Context.

- [x] **Done** with ```sleepymalc/VSCode-LaTeX-Inkscape```'s latex.hsnips. **See** also ```OrangeX4/hsnips```'s hsnips.

## 2. Excalidraw.com integration

[Excalidraw.com](https://excalidraw.com) can draw and export **.svg** figures, so:

1. Draw figures in Excalidraw, and export as **svg**.
2. Copy figures into kTestNote/Figures. The ```inscape-figures watch``` will monitor the figures and trigger Inkscape to generate **pdf_tex** and **pdf**.
3. Use ```ctrl+i e``` to trigger ```Inscape-figures edit``` to edit the figures in Inkscape(like adding some text, mathmatic formula, etc.), taking the advantage of its pdf_tex pros.
4. Add or modify code below to kTestNote.tex and rebuild it.
   ```latex
   \begin{figure}[H]
      \centering
      \incfig{font_test_of_excalidraw_svg_in_inkscape}
      \caption{Reedit Excalidraw svg in Inkscape}
      \label{fig:font_test_of_excalidraw_svg_in_inkscape}
   \end{figure}
   ```

## 3. Excel tables integration

Like Inkscpae, we can add some supports for tables.

- [x] Automation work already done.

Create tables generated from Excel using [Excel2LaTeX](https://github.com/ivankokan/Excel2LaTeX) add-in:

1. The tables can be drawn with hand in Excel: *Home->Draw outside border->Draw border*.
2. Export LaTeX code by "**Excel2LaTeX**".
3. Paste in kTestNote.tex, using packages: "**booktabs**" and "**multirow**".
4. Manually add diagbox needed, since Excel2LaTeC cannot export diagonal line, using package "**diagbox**".
5. Automation: Trigger ```ctrl+i m``` to open an excel in *Figures* with a name under cursor in the tex file, and then edit the table and export it by "Excel2LaTeX" as tex code, and paste it to the tex file.

**Attention**: There should be a "*Templates*" folder in "*Figures*", including self-customized temaplate for Excel.

## 4. XMind mindmap integration

Support import XMind exported **svg**.

1. Trigger ```ctrl+i x``` to open an xmind file in *Figures* with a name under cursor in the tex file, and then create your mindmap and export it as **svg**.
2. The ```inkscape-figures watch``` will monitor the **svg** creation and generate **pdf** and **pdf_tex** using "Inkscape".
3. Rebuild your tex to see the effect.

- [ ] **Todo**: The figure is not what it looks in original svg.
## 5. For whom not familiar with latex commands for math formula

```Mathpix``` ```EquationMaker``` ```Detexify``` ```Mathkey``` ```Mathcha``` ```Quiver```

<span style="white-space: nowrap">
<img src="https://mathpix.com/images/logo/image-logo.png" width="48">
<img src="https://static.macupdate.com/products/50374/m/equation-maker-logo.png?v=1574176973" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRt9b_RvPnFyzuoFPaycQGR46ciRmi11r1FEQ&usqp=CAU" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHtkytJ85bF9V6v2lTpoXoqcI8JVjlW2KLhLrHgeCEma6uKsyo_aOnNxaczNr5Zz6CPdo&usqp=CAU" width="48">
<img src="https://www.mathcha.io/image/notebook-icon.png" width="48">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ5rMHnsUB1YwfZkaSaac7E75_xsjqGK0BYFrLy0XHf3etrOTgGxgBbdHHU7fkoL2zIz0I&usqp=CAU" width="48">
</span>

## 6. Correcting spelling mistakes on the fly
- [ ] **Todo**.

# Credits

Thanks to **Gilles Castel** and **Pingbang Hu** for their inspiring work. Hope the workflow can become popular.

# Related Projects
1. [Gilles Castel's Main Page](https://castel.dev/)
2. [Gilles Castel's Notes](https://castel.dev/notes)
3. [Gilles Castel's inkscape-figures](https://github.com/gillescastel/inkscape-figures)
4. [Gilles Castel's shortcut manager](https://github.com/gillescastel/inkscape-shortcut-manager)
5. [Pingbang Hu's Main Page](https://www.pbb.wtf/posts/LaTeX-Inkscape)
6. [Pingbang Hu's VSCode-LaTeX-Inkscape](https://github.com/sleepymalc/VSCode-LaTeX-Inkscape)
7. [Inkscape Tutorials](https://www.youtube.com/watch?v=eyqH0IrzYLc&list=PLxtauMB7RON_2tg-mRQTuieFUr29IOKzW)
8. [WriteTex: Latex/Tex editor for Inkscape](https://inkscape.org/da/~longqi/%E2%98%85writetex)
9. [TexText: Re-editable LaTeX graphics for Inkscape](https://textext.github.io/textext/)
