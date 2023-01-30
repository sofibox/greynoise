````
greynoise --version
=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=

Info: Greynoise (greynoise) is a bash script that uses GreyNoise API to check if an IP is known for malicious activity

Version: 0.1-beta

Author: Arafat Ali | Email: arafat@sofibox.com | (C) 2019-2023

=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=
````

This bash script is used to check if an IP or domain is on a blacklist by utilizing the API from [GreyNoise](https://www.greynoise.io/).

# Setup:

Download [maxibuild](https://github.com/sofibox/maxibuild) and run the following command to install it in your system (recommended for maintainability)

`maxibuild --install greynoise --force`

or 

You can also clone this repository and run greynoise directly from the repository folder.

```
git clone https://github.com/sofibox/greynoise.git
cd greynoise
chmod +x greynoise
./greynoise --version
````

# Script configuration:

````
You can insert your API key in the script by editing the config file greynoise.conf. The script will also prompt you to enter your API key if you did not insert it in the config file.
In Greynoise, if the script will work if you do not insert your API key but the script will be limited to 10 queries per day. You can get the free API key from [here](https://greynoise.io/pricing).

You can limit the script check IP output by editing the variable GREYNOISE_OUTPUT_MAX_LIMIT from the config file greynoise.conf. Default value is 500. If the IP address has more than 500 records, the script will delete the oldest records to make sure the output is not more than 500 records.
``
````

# Script usage:

````
greynoise <+ action> <* options>, where + is required and * is optional
````

# Script documentation

List of available actions:

`check`

````
        This action is use to query the greynoise.io API. Required option is --target or --ip-address

        example:
            ./greynoise check --ip 1.2.3.4
            ./greynoise check --ip-address 1.2.3.4
            ./greynoise check --domain sofibox.com
            ./greynoise check --domain-name sofibox.com
            ./greynoise check --target 1.2.3.4
            
            
        short version:
            ./greynoise check -t 1.2.3.4
            
            Note: Greynoise does not support IPv6 yet. If an IPv6 address is used or a domain is resolved to IPv6 address, the script will return an error from API. 
            
            
        output:
            [greynoise->info]: Checking target IP address 191.15.138.10 ...
            Greynoise scan results [new]:
            -------------
            Target: 191.15.138.10
            IP address: 191.15.138.10
            Noise: true
            Riot: false
            Classification: malicious
            Name: unknown
            Link: https://viz.greynoise.io/ip/191.15.138.10
            Last seen: 2023-01-30
            Message: Success
            Last scan date: Tue Jan 31 00:12:45 +08 2023
            -------------
        
        Note: If the target IP address is not found in the GreyNoise database, the script will return 404 response code following output:
        
            [greynoise->info]: Checking target IP address 191.15.13.2 ...
            [greynoise->scan]: Error, API return unsuccessful HTTP code 404
            [greynoise->scan]: Error details:
            {
              "ip": "191.15.13.2",
              "noise": false,
              "riot": false,
              "message": "IP not observed scanning the internet or contained in RIOT data set."
            }
            
        So, if you use --scripting option, the script will return an exit code 254 with a value `error` in the terminal.
````

Other optional options:
````
-h, --help
    This option is use to display the help message for the script
    
-v, --verbose
    This option is use to enable verbose output. You can use this option multiple times to increase the verbosity level (eg: -vvv)
    
    eg:
        ./greynoise check --ip-address 1.2.3.4 --verbose or ./greynoise check --ip-address 1.2.3.4 -v
    
-s, --scripting
    This option is use to enable scripting mode. When this option is enabled, you can the get a script status code or print the short result
   
    eg: 
        ./greynoise check -t 191.15.138.10 --scripting
        echo $?
        malicious
        
        Note: The return value above is malicious (with return code 1). It means the target IP address is malicious. The complete list of return codes are as follow:
         
         clean - (return 0), malicious (return 1), unknown (return 2), null (return 3).   

-j, --json
    This option is use to enable json output. When this option is enabled, the script will output the result in json format. 
    Note: If the target IP address is not found in the GreyNoise database, the script will return an error message in json format.
    
    eg:
        ./greynoise check --ip-address 1.2.3.4 --json
        
    output (sample):
    
        [greynoise->info]: Checking target IP address 165.227.236.118 ...
        Greynoise scan results [new]:
        -------------
        {
          "ip": "165.227.236.118",
          "noise": true,
          "riot": false,
          "classification": "unknown",
          "name": "unknown",
          "link": "https://viz.greynoise.io/ip/165.227.236.118",
          "last_seen": "2023-01-30",
          "message": "Success"
        }
        -------------
    
    
-c, --config
    This option is use to specify the config file to use. If this option is not specified, the script will use the default config file (greynoise.conf) in the same directory as the script.
    The script will create a new config file if it does not exist or it contains invalid data (you will be prompted to perform this action).
    
    eg:
        ./greynoise check --ip-address 1.2.3.4 --config /path/to/config/file
        
-o, --output
    This option is use to specify the output file to use. If this option is not specified, the script will use the default output file (greynoise_check_output.txt) in the same directory as the script.
    
    eg:
        ./greynoise check --ip-address 1.2.3.4 --output /path/to/output/file
        
-k, --cache
    This option is use to use previous scan output as cache. If this option is not specified, the script will perform a new scan.
    
    eg:
        ./greynoise check --ip-address 1.2.3.4 --cache

````
