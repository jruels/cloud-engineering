# Use GitLab to setup a CICD pipeline

GitLab has shared runners that are used to execute CICD pipelines. To avoid any abuse, they require the user to provide a credit card for validation purposes. **There are no charges!**

As an option, you can install and use your own runner. 

In this lab, you'll: 

* Create a Cloud9 instance
* Install and register a `gitlab-runner`
* Create a new project
* Create a CICD configuration file
* Commit the configuration file
* Review the pipeline status



## Create a Cloud9 environment

This lab requires a Cloud9 environment, as CloudShell lacks the required disk space.

To create a Cloud9 environment:

1. In the top right make sure the region is set to **Oregon**
2. Search for and open Cloud9 at the top of the AWS Console.
3. Create a new environment with the following settings:
4. Name: `gitlab`
5. Instance type: `t2.micro`
6. Change **Platform** to `Ubuntu Server 18.04 LTS`
7. Click “Create”

Open the Cloud9 IDE



### Create a new project

In GitLab create a new private project named `CI Test`. 

### Install and register GitLab runner

In Cloud9, install the latest `gitlab-runner`

Install the GitLab package repository

```bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```



Install the latest `gitlab-runner` package

```bash
sudo apt-get install gitlab-runner
```



Confirm it was installed 

```bash
sudo gitlab-runner status
```

If the status shows `running` , continue with the lab, otherwise ask the instructor for assistance.

### Register a specific runner dedicated to your project

In your **CI Test** project, in the left navigation pane, click **Settings > CI/CD**.

Scroll down to the **Runners** section. Click the **Expand** button next to that section.

Within the **Specific runners** section, navigate to **Set up a specific runner manually**.

Copy the URL in step 2, labeled **Register the runner with this URL**.

Also copy the **registration token**, and save it for later in this lab.

Disable **shared runners**

In Cloud9, register the runner

```bash
sudo gitlab-runner register
```

When prompted, paste the URL you just copied. 

Paste the **registration token** copied earlier.

Accept the defaults for the remaining questions, until it gets to the **executor**. Select `shell` for the **executor**.

Confirm you `gitlab-runner` is set up correctly

```bash
sudo gitlab-runner list
```

You will see a new `gitlab-runner` with the name you gave it, and the `shell` executor.

### Create CI configuration file

In GitLab perform the following:

* Create a new file named `.gitlab-ci.yml`

* Select the file and apply the **General > Bash** template 

  In the editor, delete all lines above the `build1:` line and below the `- echo "For example run a test suite` line. This will leave you with two sections of code, which define the **build1** and **test1** jobs.

* Define **build** and **test** stages by adding these 3 lines at the top of the file. The `stages` keyword must be flush left and the stage names must be indented by 2 spaces.

  ```yaml
  stages:
    - build
    - test
  ```

* Leave the default values for the **Commit message** and **Target Branch** fields, and select **Commit changes**.

* After committing the file, visit **CI/CD > Pipelines** to review the status.

* Confirm it used your local runner, by clicking on the pipeline status, and the **build** widget. In the top right corner you'll see **Runner** with the name you assigned.



