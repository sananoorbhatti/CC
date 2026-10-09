Assignment 01: Git, Gitea, GitHub, Git LFS, and GitHub Pages
Deadline: 10 October 2026
Total marks: 10
Submission repository: CC
Submission directory: Assignments/Assignment01
Screenshot directory: Assignments/Assignment01/screenshots
Required assignment files: 3
Required screenshots: 12

Complete all practical work using the environment specified for each task. Submit the required evidence to your existing public GitHub repository named exactly CC using the directory structure shown below.

CC/
└── Assignments/
    └── Assignment01/
        ├── Assignment01.md
        ├── Assignment01_Solution.docx
        ├── Assignment01_Solution.pdf
        └── screenshots/
            ├── task1_gitea_running.png
            ├── task1_gitea_push.png
            ├── task1_gitea_repository.png
            ├── task2_remotes.png
            ├── task2_github_push.png
            ├── task2_github_repository.png
            ├── task3_lfs_setup.png
            ├── task3_lfs_files.png
            ├── task3_lfs_push.png
            ├── task4_pages_repository.png
            ├── task4_pages_deployment.png
            └── task4_portfolio_live.png
Assignment Objective
In this assignment, you will install and run Gitea on your Ubuntu server, create and manage a Git repository from the same server, push that repository to Gitea and GitHub, manage large files with Git LFS, and publish a portfolio or CV with GitHub Pages.

By the end of this assignment, you will be able to:

Install, run, and access a Gitea service on an Ubuntu server.
Create and manage a Git repository from an Ubuntu server.
Work with multiple Git remotes.
Authenticate safely without exposing credentials.
Track and push large files with Git LFS.
Publish a static portfolio or CV with GitHub Pages.
Required Working Environments
Use the following environment for each type of work:

Work	Required environment
Install and run the Gitea service	Ubuntu server
Perform Git terminal work for Tasks 1, 2, and 3	Ubuntu server
Use Gitea and GitHub web pages	Web browser
Store assignment evidence	Existing GitHub repository named CC
Complete the Gitea installation and all Git terminal work for Tasks 1, 2, and 3 on your Ubuntu server. Use the existing compose.yaml supplied in the instructor's Gitea repository without changing its server settings.

Your Ubuntu terminal prompt must show your registered GitHub username and the hostname ubuntu:

<github-username>@ubuntu:~$
The username comparison is case-insensitive. For example, StudentName on GitHub and studentname on Ubuntu are treated as the same username when the spelling is otherwise identical.

Task List
Getting Started
Task 1: Install Gitea and Push a Repository from the Ubuntu Server
Task 2: Push the Same Repository to GitHub
Task 3: Track Three Large Files with Git LFS
Task 4: Create a Portfolio or CV with GitHub Pages
Assignment Summary
Final Submission Checklist
Getting Started
Inside your existing CC repository, create the assignment screenshot directory:

mkdir -p Assignments/Assignment01/screenshots
Confirm that your Ubuntu server terminal prompt follows this pattern:

<github-username>@ubuntu:~$
Confirm that Git and Git LFS are installed on the Ubuntu server. Docker and Docker Compose will be installed in Task 1.

Read all four tasks before beginning the assignment.

Create the following two solution files inside Assignments/Assignment01/ and add your evidence to them as you complete each task:

Assignment01_Solution.docx
Assignment01_Solution.pdf
Task 1: Install Gitea and Push a Repository from the Ubuntu Server
Step 1.1: Install and Run Gitea on the Ubuntu Server
Connect to your assigned Ubuntu server.

Update the Ubuntu package index and install Docker Engine and curl. On Ubuntu, the required package name is docker.io:

sudo apt update -y
sudo apt install -y docker.io curl
Create the system-wide Docker CLI plug-in directory and download the Docker Compose plug-in for an x86_64 Ubuntu server:

sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
Start the Docker service and add your current Ubuntu account to the docker group:

sudo systemctl start docker
sudo usermod -aG docker $USER
exit
Completely sign out of the Ubuntu server and reconnect so that the new group membership takes effect. Opening only a new terminal tab is not sufficient.

Verify the Git, Docker, and Docker Compose installations:

ssh <github-username>@<ubuntu-server-ip>
git --version
docker --version
docker compose version
Do not continue until all three commands display their installed versions.

Clone the instructor-provided Gitea setup repository on the Ubuntu server:

cd ~
git clone https://github.com/WaqasSaleem97/Gitea.git
cd Gitea
Use the existing compose.yaml without changing its server URL or port settings. Start Gitea and PostgreSQL:

docker compose up -d
Confirm that both containers are running:

docker compose ps
Confirm that Gitea responds locally on port 3000:

curl -sS -o /dev/null -w "Gitea HTTP status: %{http_code}\n" http://127.0.0.1:3000/
Display the Ubuntu server's IP address:

ip addr
Open Gitea in a web browser using the server address provided for your lab:
http://<ubuntu-server-ip>:3000/
On the initial Gitea installation page, use these database settings:
Setting	Required value
Database type	PostgreSQL
Database host	db:5432
Database username	gitea
Database password	gitea
Database name	gitea
Server domain	<ubuntu-server-ip>
Gitea base URL	http://<ubuntu-server-ip>:3000/
Create your Gitea administrator account and complete the installation. Do not show any password or token in a screenshot.
The screenshot must show your <github-username>@ubuntu prompt, the docker compose ps command, both Gitea and PostgreSQL running, and the successful Gitea HTTP status.

📸 Screenshot required immediately after this step: Save it as task1_gitea_running.png.

Step 1.2: Create and Push the Initial Repository
In Gitea, create an empty public repository named exactly Assignment01.

Do not initialize it with a README, .gitignore, or license.

On the same Ubuntu server, create a separate local repository outside both the Gitea directory and the existing CC repository:

cd ~
mkdir Assignment01
cd Assignment01
git init
git branch -M main
Create a README.md containing:

your full name; and
your registration number.
Commit the README:

git add README.md
git commit -m "Add student information"
In Gitea, generate a personal access token:

Open your profile menu and select Settings.
Open Applications and then Manage Access Tokens.
Use ubuntu-server-token as the token name.
Give the token read-and-write repository permission (write:repository).
Copy the generated token. Do not place it in a command, remote URL, or screenshot.
Because Gitea and Git are running on the same Ubuntu server, add the local Gitea address as the gitea remote. Include your Gitea username, but do not include the personal access token:

git remote add gitea http://<gitea-username>@127.0.0.1:3000/<gitea-username>/Assignment01.git
The git remote add command only records the repository address; it does not contact Gitea or perform authentication.

Verify that the remote URL contains your username but does not contain a password or token:

git remote -v
Push the initial commit:

git push -u gitea main
When Git displays the password prompt, paste the Gitea personal access token and press Enter:

Password for 'http://<gitea-username>@127.0.0.1:3000':
The token is used as the password for this HTTP Git operation. Nothing will appear while the token is being pasted or typed; this is normal.

Never use a URL such as http://username:token@127.0.0.1:3000/.... Embedding the token would save it in shell history and .git/config, and it would be displayed by git remote -v.

The terminal must show the git push command, the gitea remote, a successful push, and your <github-username>@ubuntu prompt. The token must not be visible.

📸 Screenshot required immediately after this step: Save it as task1_gitea_push.png.

Step 1.3: Verify the Gitea Repository
Open the Assignment01 repository in Gitea.
Confirm that the repository owner and repository name are visible.
Confirm that the repository is public.
Confirm that the rendered README.md shows your full name and registration number.
Keep the Gitea URL visible in the browser.
📸 Screenshot required immediately after this step: Save it as task1_gitea_repository.png.

Task 1 result: You installed and ran Gitea on your Ubuntu server, created a separate local repository on that server, and pushed it to Gitea using HTTP over the server's local loopback interface.

Task 2: Push the Same Repository to GitHub
Step 2.1: Create the GitHub Repository and Verify Both Remotes
Create an empty public GitHub repository named exactly Assignment01.

Do not initialize it with a README, .gitignore, or license.

On the Ubuntu server, open the same local Assignment01 repository used in Task 1.

Add GitHub as the second remote:

cd ~/Assignment01
git remote add github https://github.com/<your-github-username>/Assignment01.git
Verify both remotes:

git remote -v
The output must show both gitea and github, including their fetch and push URLs. No token or password may appear in either URL.

📸 Screenshot required immediately after this step: Save it as task2_remotes.png.

Step 2.2: Push to GitHub
Push the main branch to the GitHub remote:

git push -u github main
The terminal must show the command, the GitHub remote, a successful result, and your <github-username>@ubuntu prompt.

📸 Screenshot required immediately after this step: Save it as task2_github_push.png.

Step 2.3: Verify the GitHub Repository
Open the following repository page:

https://github.com/<your-github-username>/Assignment01
Verify that:

the repository owner is your GitHub account;
the repository name is Assignment01;
the repository is public; and
the rendered README.md shows your full name and registration number.
Keep the repository owner, repository name, and rendered README visible.

📸 Screenshot required immediately after this step: Save it as task2_github_repository.png.

Task 2 result: The same local repository is connected to both Gitea and GitHub and is available publicly on GitHub.

Task 3: Track Three Large Files with Git LFS
Step 3.1: Configure Git LFS
On the Ubuntu server, open the local Assignment01 repository, initialize Git LFS, confirm its version, and configure tracking for .bin files:

cd ~/Assignment01
git lfs install
git lfs version
git lfs track "*.bin"
cat .gitattributes
The terminal must show:

successful Git LFS initialization;
the Git LFS version;
the git lfs track "*.bin" command; and
the resulting *.bin rule in .gitattributes.
📸 Screenshot required immediately after this step: Save it as task3_lfs_setup.png.

Step 3.2: Create and Verify Three LFS Files
Create three different files, each larger than 100 MiB. For example:

truncate -s 101M large-file-1.bin
truncate -s 102M large-file-2.bin
truncate -s 103M large-file-3.bin
Stage .gitattributes and the three files:

git add .gitattributes large-file-1.bin large-file-2.bin large-file-3.bin
Verify that all three files are tracked by Git LFS and display their sizes:

git lfs ls-files --size
The output must clearly list all three files, and every file must be larger than 100 MiB.

📸 Screenshot required immediately after this step: Save it as task3_lfs_files.png.

Step 3.3: Commit and Push the LFS Files
Commit and push the files to the GitHub Assignment01 repository:

git commit -m "Add three files using Git LFS"
git push github main
Capture the first successful upload. The terminal should show the LFS object upload completing successfully, preferably 100% (3/3).

📸 Screenshot required immediately after this step: Save it as task3_lfs_push.png.

Task 3 result: Three files larger than 100 MiB are tracked with Git LFS and pushed to the GitHub Assignment01 repository.

Task 4: Create a Portfolio or CV with GitHub Pages
Step 4.1: Create and Push the Portfolio Repository
Create a new public GitHub repository named:

<your-github-username>.github.io
Create a portfolio or CV using HTML and CSS.

Include at least these files:

index.html
styles.css
The portfolio must visibly contain these sections:

About Me
Education
Skills
Projects
Commit and push the source files to the main branch.

Open the GitHub repository page. Keep the repository owner, repository name, index.html, and styles.css visible.

📸 Screenshot required immediately after this step: Save it as task4_pages_repository.png.

Step 4.2: Publish with GitHub Pages
Open the portfolio repository on GitHub.
Go to Settings → Pages.
Under Build and deployment, select Deploy from a branch.
Select the main branch and the / (root) folder, then save.
Wait until GitHub reports that the site is published or live.
The page must show GitHub Pages, a successful published or live status, and your <github-username>.github.io URL.

📸 Screenshot required immediately after this step: Save it as task4_pages_deployment.png.

Step 4.3: Verify the Live Portfolio
Open the following URL in your browser:

https://<your-github-username>.github.io/
Confirm that the site loads successfully. Keep the complete github.io URL and the portfolio content visible, including at least one required section such as About Me, Education, Skills, or Projects.

📸 Screenshot required immediately after this step: Save it as task4_portfolio_live.png.

Task 4 result: Your portfolio or CV is stored in a public GitHub repository and published as a live GitHub Pages site.

Assignment Summary
In this assignment, you:

installed and ran a Gitea service on an Ubuntu server;
performed Git work from an Ubuntu server with your GitHub identity visible;
pushed the same local repository to Gitea and GitHub using two remotes;
configured Git LFS and pushed three large files; and
created and published a portfolio or CV with GitHub Pages;
prepared a complete Word solution; and
exported the completed Word solution as a readable PDF.
Final Submission Checklist
Before submission, confirm that:

 I completed the assignment individually.
 Assignment01.md is inside CC/Assignments/Assignment01/.
 Assignment01_Solution.docx is inside CC/Assignments/Assignment01/ and opens correctly.
 Assignment01_Solution.pdf is inside CC/Assignments/Assignment01/, matches the Word solution, and opens correctly.
 My CC repository contains Assignments/Assignment01/screenshots/.
 All 12 required screenshots are present and readable.
 Every screenshot was captured immediately after the requested step.
 Every screenshot was created from my own work.
 My GitHub username is visible in every Ubuntu server terminal screenshot.
 Every terminal screenshot shows the required command and its relevant result.
 Every browser screenshot shows the required repository, URL, page, or status.
 No screenshot contains a token, password, private key, or other credential.
 My CC, GitHub Assignment01, and <username>.github.io repositories are public.
 My Gitea Assignment01 repository was created and pushed from the Ubuntu server.
 All repositories use the main branch.
 I pushed the complete Assignments/Assignment01 directory before the deadline.
 I uploaded all three required assignment files to Google Classroom.
 I submitted the three required links on Google Classroom.
Academic Integrity
Marks are awarded for completing and demonstrating the required process, not only for showing a final result. You may be asked to explain or demonstrate any submitted step. Copied work, copied screenshots, or evidence that does not belong to the submitting student will not receive credit.

Official Documentation
Creating a GitHub repository
About large files on GitHub
Git Large File Storage
Creating a GitHub Pages site
Installing Gitea with Docker
Gitea API tokens and repository permissions
