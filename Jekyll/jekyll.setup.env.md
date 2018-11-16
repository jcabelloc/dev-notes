# Jekyll on Windows
Reference: https://jekyllrb.com/docs/installation/windows/

### First Steps
* Download and Install a Ruby+Devkit version from RubyInstaller Downloads. https://rubyinstaller.org/downloads/
* Use default options for installation. 
* Open a Git Bash. Install Jekyll and Bundler via: 
```bash
$ gem install jekyll bundler
```

* Check if Jekyll installed properly: 
```bash
$ jekyll -v
```

### Requirements
Reference: https://jekyllrb.com/docs/installation/

* Ruby version 2.2.5 or above
``` bash
$ ruby -v
```
* RubyGems 
```bash
$ gem -v
```
* GCC and Make
```bash
$ gcc -v
$ g++ -v 
$ make -v
```

### Add make to git bash
* Reference https://gist.github.com/evanwill/0207876c3243bbb6863e65ec5dc3f058
* Go to ezwinports: https://sourceforge.net/projects/ezwinports/files/
* Download make-4.1-2-without-guile-w32-bin.zip (get the version without guile).
* Extract zip.
* Copy the contents to your C:\Program Files\Git\mingw64 merging the folders, but do NOT overwrite/replace any existing files.
* Check if make installed properly
```bash
$ make -v
```