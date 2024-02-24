There is currently a treesitter for systemverilog aptly name, "tree-sitter-verilog". 

I started with using that and quickly realized that the highlighting is messed up. I tried debugging it, but showed up with nothing.


Turns out, there is a branch that has fixed that issue already, but for some reason, the owner hasn't merged the PR yet.
So i tried cloning the branch, compile it and googled how to load a custom parser. Parser works but no highlights.

// HERE IS THE REPO BTW...
https://github.com/gmlarumbe/tree-sitter-verilog.git

As an initial noob shitty fix, I copied the query files from the offical verilog_ts, and just directly slapped that onto 
the packer directories, so the nvim-treesitter will see these files as query files for the custom parser.

For some reason that worked, so to document that I made this repo, (FOR MY OWN SAKE, since i forget stuff). 
