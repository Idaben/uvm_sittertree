UVM is a not a language, and so this is not a UVM tree sitter repo. It is a guide for myself, when I forget how to put the better working systemverilog tree sitter parser into my neovim setup. 






There is currently a treesitter for systemverilog aptly name, "tree-sitter-verilog". 

I started with using that and quickly realized that the highlighting is messed up. I tried debugging it, but showed up with nothing.


Turns out, there is a branch that has fixed that issue already, but for some reason, the owner hasn't merged the PR yet. So i tried cloning the branch, compile it and googled how to load a custom parser. Parser works but no highlights.

// HERE IS THE REPO BTW...
https://github.com/gmlarumbe/tree-sitter-verilog.git

As an initial noob shitty fix, I copied the query files from the offical verilog_ts, and just directly slapped that onto the packer directories, so the nvim-treesitter will see these files as query files for the custom parser.

For some reason that worked, so to document that I made this repo, (FOR MY OWN SAKE, since i forget stuff). 




In treesitter.lua, put this block to load the branch with the fix... We can just probably use the git repo here but replace
the name of the parser with the original (tree-sitter-verilog) dunno if that wil conflict with anything but we'll see. (i named it idaben_uvm_ts, so it will not conflict with the orignal when I was comparing codes between the two...)

```
local parser_config = require "nvim-treesitter.parsers".get_parser_configs()
   parser_config.idaben_uvm_ts = {
     install_info = {
       url = "~/Coding/tree-sitter-verilog/", -- local path or git repo
       files = {"src/parser.c"}, -- note that some parsers also require src/scanner.c or src/scanner.c
       -- optional entries:
      branch = "master", -- default ranch in case of git repo if different from master
       generate_requires_npm = false, -- if stand-alone parser without npm dependencies
       requires_generate_from_grammar = false, -- if folder contains pre-generated src/parser.c
     },
     filetype = "systemverilog", -- if filetype does not match the parser name
   }
```


note : if the name of the parser is changed, (such as the snip of code here, "idaben_uvm_ts"), then you need to go in to the grammar.js of the source and replace the name of the parser there too. Then you would need to recompile, so you'll have the proper .so file that will be used by nvim_treesitter.

hack manually :
copy the idaben_uvm_ts.so file here, and go to the working directory of nvim_treesitter. (something like, site/pack/packer, we were using packer at this point, so if I changed my plugin manager, then this would also change). In there you will see a folder named "parser", that will contain the .so files of all the languages that you installed based in the treesitter.lua. Copy idaben_uvm_ts.so here. In that same main directory, there will be a folder called queries or query, mkdir idaben_uvm_ts folder, and copy the scheme files in there. Also don't forget to put the code snip in the treesitter.lua and update the url or path (but do not install!!!!, since this will replace the hacked .so). Everything should work at this point. Opening a .sv file would have the proper highlighting and running :InspectTree or :TSPlaygroundToggle should show a beautiful node tree.
