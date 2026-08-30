# Question

There is a Git server utilized by the Nautilus project teams. Recently, a new developer named Jon joined the team and needs to begin working on a project. To begin, he must fork an existing Git repository. Follow the steps below:

- Click on the Gitea UI button located on the top bar to access the Gitea page.
- Login to Gitea server using username jon and password Jon_pass123.
- Once logged in, locate the Git repository named sarah/story-blog and fork it under the jon user.

Note: For tasks requiring web UI changes, screenshots are necessary for review purposes. Additionally, consider utilizing screen recording software such as loom.com to record and share your task completion process.

# Step by Step Solution

1. **Open Gitea UI:**

Access Gitea interface.

Click the Gitea UI button located on the top bar of your lab interface to open the Gitea web portal in your browser.

2. **Sign In to Gitea:**

Credentials: jon / Jon_pass123.

Click the Sign In button at the top right corner.

Enter the credentials:

Username or Email Address: jon
Password: Jon_pass123
Click Sign In.

3. **Locate the Target Repository:**

Search for target repository.

Use the top search bar or navigate to Explore / Repositories.

Find and open the repository named `sarah/story-blog` (or access it directly via the URL structure `http://<gitea-host>/sarah/story-blog`).

4. **Fork the Repository:**

Execute fork action.

On the main page of the `sarah/story-blog` repository, click the Fork button located near the top right section.

In the fork options screen:

Ensure Fork Owner is set to jon.

Leave the Repository Name as story-blog (or default).

Click Fork Repository.

5. **Confirm Fork Ownership:**

Verify completion.

Ensure you are redirected to your newly created fork, located under `jon/story-blog`. Take a screenshot of the completed page if required for submission.