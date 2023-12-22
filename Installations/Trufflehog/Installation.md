## Install pip

    sudo apt install python3-pip
    
## Install trufflehog

    pip install truffleHog


## Add the Directory to PATH

To make the trufflehog command accessible from anywhere in your terminal, you can add the directory containing the script to your system's PATH.
You can do this by adding the following line to your shell profile configuration file (e.g., .bashrc, .zshrc, or similar):

    export PATH=$PATH:/home/ubuntu/.local/bin

After adding the line, save the file and either restart your terminal or run source ~/.bashrc (or the appropriate file for your shell) to apply the changes immediately.

Use the Full Path:
Alternatively, you can run the trufflehog command using its full path:

    /home/ubuntu/.local/bin/trufflehog

## Suppress the Warning:

If you're not concerned about the warning, you can suppress it by using the --no-warn-script-location flag when you run TruffleHog:

    trufflehog --no-warn-script-location

Keep in mind that adding the directory to your PATH is the most convenient option, as it allows you to use the trufflehog command without specifying the full path every time.

## Run TruffleHog:

In the build step or pipeline stage, run TruffleHog by executing the following command:

    trufflehog --regex --entropy=False --json .

    --regex flag enables regular expression searching.
    --entropy=False flag disables entropy checking to speed up the scan (optional).
    --json flag formats the output as JSON.

The . at the end specifies that TruffleHog should scan the current directory and its subdirectories.
