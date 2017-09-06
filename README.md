Not the Blog
============

Go to openmonstervision.github.io/blog

Handy Commands:
--------------

    # build... and for publishing...?
    #
    hugo --theme=hugo-theme-cactus-plus -D -E -F

    # new post
    #
    hugo new posts/<post-name>.md

    # localhost this shit
    #
    hugo server -w # in ./config.toml -- this needs to be true --> buildDrafts


Git Submodule Notes:
-------------------

    # /.gitmodules
    #
    [submodule "BluestNight"]
    	path = themes/BluestNight
    	url = https://gitlab.com/Shadow53/BluestNight.git

    [submodule "hugo-theme-cactus-plus"]
    	path = themes/hugo-theme-cactus-plus
    	url = https://github.com/nodejh/hugo-theme-cactus-plus.git


    # Update the any git submodules or if you don't have them pull them.
    #
    git submodule update --init --recursive
