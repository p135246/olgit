# Olgit 1.0

## Description

A BASH script to synchronize a Git repository with Overleaf using [overleaf-sync](https://github.com/moritzgloeckl/overleaf-sync) of Moritz Glöckl written in Python.

## Usage

Olgit synchronize a local git repository `DIR` with a project named `PROJ` on Overleaf.
It creates a new git repository `DIR/.olgit` where a local copy of `PROJ` is kept.
The workflow is to first pull `PROJ` from Overleaf to `DIR/.olgit` by running `olgit pull PROJ DIR`, then merge it to `DIR` using `olgit merge DIR`, and finally push `DIR` to `PROJ` on Overleaf by running `olgit push PROJ DIR`.
The merge is implemented as `git pull OLDIR/.olgit DIR --allow-unrelated-histories`.
The script also creates an authentification cookie `DIR/.olauth` and a list of ignored files `DIR/.olignore` needed by `ols` (overleaf-sync).
The cookie has to be renewed time to time (delete it and run olgit pull).

## List of commands and their technical description

* `olgit`
   
          Displays the list of commands.
* `olgit pull PROJ DIR`

          Download `PROJ` from Overleaf to `DIR/.oldir` and commit.
          If `DIR/.oldir` does not exist, it will be created and a repository will be initialized therein.
          If `DIR/.olauth` does not exist, the user will be asked to log in Overleaf.
          The authentification cookie will be downloaded by `ols` and stored in `DIR/.olauth`.
          It does not contain user's credentials.
* `olgit merge DIR`:
          
          Just runs `git pull OLDIR/.olgit DIR --allow-unrelated-histories` and lets the user to resolve possible conflicts in the git way.
          It is generally recommended to commit `DIR` first before running this command.
          Git then overwrites the content of `DIR` with the content of `DIR/.olgit`, copies the entire history of `DIR/.olgit` to `DIR` branching from a common ancestor, and let user to resolve conflicts and create a merge commit.
* `olgit push PROJ DIR`:

          Calls `ols` to upload tracked files in `DIR` to `PROJ` on Overleaf.
          By design of `ols`, a file `DIR/.olignore` with the list of all ignnored files, i.e., the untracked files, has to be created.
          
## TODO

1. Rewrite olgit in Python without using `olsync.py` at all.
   Only the Overleaf API wrapper `olclient.py` (based on website scraper for python `beautifulsoup`) should be used.
2. Add an option to specify location of the `.olauth` file.
3. Replace the `.olignore` concept with a list of files to synchronize. 
