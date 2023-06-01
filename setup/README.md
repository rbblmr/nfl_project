Prerequisites 
1. Create a GCP account with your Google email address

2. Setup your project named `nfl-project-de`. Take note of your `project ID`.

3. Enable the following APIs in the console:

    - https://console.cloud.google.com/apis/library/iam.googleapis.com
    - https://console.cloud.google.com/apis/library/iamcredentials.googleapis.com
    - https://console.developers.google.com/apis/api/dataproc.googleapis.com/


4. To run, go into the root of project directory `~/nfl_project/`. Make sure the folder follows the `nfl_project` name:

  1. Create a GCP VM using the `setup_vm.sh`

      ```
      ./setup_vm.sh
      ```
- You should now be in the VM's shell
  2. Clone the repo and move to the repo directory to use the Makefile

      ```
      git clone https://github.com/rbblmr/nfl_project.git
      ```

  3. Modify the .env file depending on your setup, modify the GOOGLE_APPLICATION_CREDENTIALS in the Makefile

  4. Run this to have `make` avaiable as a command in the VM:

      ```
      sudo apt install make
      ```
  
  5. Install prerequisites: (CHECK IF YOU need to restart docker again on new ssh)
    
      ```
      make prerequisites
      ```

  6. Exit the shell and start an SSH session again

      ```
      ssh $GCP_VM.$GCP_ZONE.$GCP_PROJECT_ID
      ```
  
  
  7. Initialize gcloud:

      ```
      make gcloud-initialize
      ```
  
  8. Create terraform infrastructure:

      ```
      make terraform-infra
      ```

  9. Setup airflow:

      ```
      make airflow-setup
      ```

  10. Initialize gcloud within the airflow worker:

      ```
      make airflow-gcloud-init
      ```
  
  12. Forward the 8080 port to access airflow in your browser. The easiest way to do this is in VSCode. You already have your config file when you used `gcloud compute config-ssh` in `setup_vm.sh`.
    a. Make sure you've installed `Remote-SSH` extension in VSCode
    b. Open a Remote Window and select `Connect to Host`
    c. Choose your $GCP_VM.$GCP_ZONE.$GCP_PROJECT_ID
    d. In the Terminal panel, choose ports and do port forwarding.
    e. You can now access the airflow UI in your browser

      or
  
      Open another terminal session:

      ```
      ssh -L 8080:localhost:8080 $GCP_VM.$GCP_ZONE.$GCP_PROJECT_ID
      ```
  
  13. In the airflow UI, enter the default credentials `username: airflow` `password:airflow` you should see two DAGs. Run them.