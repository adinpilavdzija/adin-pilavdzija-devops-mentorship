# 01 Start using git and GitHub

## Task

You need to create public GitHub repository inside your own GitHub account called: `firstname-lastname-devops-mentorship`.  
You need to create `.gitignore` and `README.md` files and push those files to GitHub.

Please pay attention on the following:

- Your repository should have two branches
    - `main`
    - `development`

Direct commits to `main` branch should be forbidden.

All changes needs to be pushed to `development` branch first and then you need to make Pull Request from `development` to `main` branch.

Once when you do that post your pull request as a comment to this ticket for approval.

Pull request will be approved from @allops-solutions/mentors group. Before sending Pull Request for review please add them as a collaborators to your GitHub project.

## Task solution 

Kreiran GitHub repozitorij `adin-pilavdzija-devops-mentorship` sa `.gitignore` i `README.md` fajlovima. 

Direktno komitanje na main branch zabranjeno dodavanjem `Branch protection rule`-a.  
Repository’s Settings -> Branches -> Add rule -> za "Branch name pattern" upisati `main` -> odabrati checkbox za `Require a pull request before merging`.

---

Kreiranje ssh ključeva:

```
$ ssh-keygen -t rsa #komanda za generisanje ssh privatnog i javnog kljuca 
$ cat .ssh/id_rsa.pub # ispis javnog kljuca
``` 

Na GitHub profilu -> Settings -> SSH and GPG keys -> New SSH key -> paste javni kljuc.