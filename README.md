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
