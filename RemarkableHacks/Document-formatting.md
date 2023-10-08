<h1>List and guide for current remarkable hacks</h1>
Currently all your scripts, backups etc. are located in /Home/Documents/Remarkable2

<h3>  PDF to remarkable notebook </h3>
Dependencies: 
- drawj2d 

<b>Use:</b> 
drawj2d -Trmapi input.pdf 

This creates .zip which is sent to remarkable using ./rmapi 

<h3> Multiple-page PDF to remarkable notebook </h3>
<a>https://github.com/JCN-9000/pdf2rmnotebook</a>
Use pdf2rmnotebook.sh (located in earlier stated directory)
Dependencies: 
- drawj2d
- pdfinfo 

Use: 
CLI, ./pdf2rmnotebook input.pdf 

Outputs "Notebook.zip" - rename and send via. rmapi. 
#todo: shell-script for more appropriate naming than Notebook.zip

<h3>Scientific papers (or any pdf) to remarkable</h3>
https://github.com/GjjvdBurg/paper2remarkable
(pacman -S pdftk ghostscript poppler -> pip install paper2remarkable)
Dependencies: 
- Stated above

Use: 
Implemented getPaper.sh in earlier described dir. 
./getPaper.sh 

log: 04/09/2022: updated ~/.paper2remarkable.yml with config for sourcing rmapi as it did not work beforehand. 

If formatting is weird, disable -r (right move) flag by just calling p2r url 

When sending local files, note that -r is NOT enabled. Simply use p2r -r path/to/file for locals. 

<b> Important: May be used for generating pdfs locally with blank page after each page (good for school-assignment work), which then followingly may be converted to notebook-format using pdf2rmnotebook.sh</b>




<h1> Remarkable Templates </h1>
copying all templates
`scp -r root@10.11.99.1:/usr/share/remarkable/templates /home/max/Documents/remarkable_templates`



