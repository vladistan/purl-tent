
# PURL-Tent

**PURL-Tent** is temp and flexible redirection service
designed to provide a quick fix for broken links in the semantic
web. This project serves as a makeshift "tent" to replace purl.org
while the folks at the Internet Archive are working on fixing the
issues.

PURL-Tent is needed because purl.org plays a crucial role in the
semantic web, serving as a stable URL redirection service that many
ontologies rely on as their URL base. With purl.org currently down,
numerous imports are broken, causing widespread issues.

One notable ontology that heavily relies on purl.org is Dublin Core,
which is widely used across the semantic web community. As a result,
a significant number of imports are currently broken due to the
purl.org outage.

To address this problem, PURL-Tent was quickly put together as a
temp measure to provide a makeshift solution until purl.org
is back up and running. This project aims to minimize the impact
of the outage and ensure that the semantic web can continue to
function as smoothly as possible during this time.

## Overview

PURL-Tent is a lightweight Docker container based on NGINX to
handle HTTP redirects. It allows users to set up a temp
redirection service by mapping PURL paths to new resource locations.


## Getting Started

### Prerequisites

- Some service that can run Docker containers,  has a stable IP address that you can put into your hosts file.
- Docker or another tool that lets you build docker images.  You can use mine `vladistan/purl-tent:latest` but then you are stuck with redirects that I set up.

### How to use

1. Clone the repository:
   ```bash
   git clone https://github.com/vladistan/purl-tent.git
   cd purl-tent
   ```

2. Edit the `nginx.conf` file to add your redirection rules.

2. Build the Docker image:
   ```bash
   docker build -t purl-tent .
   ```

3. Run the Docker in your infrastructure

4. Edit your /etc/hosts on Mac or Linux or c:\Windows\System32\drivers\etc\hosts on Windows to add an entry like this:
   
   ```
   <your-docker-host-ip> purl.org
   ```

5.  PROFIT!


### Finding the Redirect Location

With purl.org currently down, it can be challenging to determine
where to redirect specific URLs. 

There is a bit of chicken and egg problem here. To configure the 
redirection we need to know where to redirect to.  Normally one would
use purl.org to find this information.  However, because purl.org is
and we are trying to replace it we can't use it to find this information.
Fortunately, thanks to the good folks at the Internet Archive, the
Wayback Machine, has been partially restored, we can now use it to find the 
redirection targets.

For example, let's consider the Dublin Core Terms namespace, which
uses the URL `http://purl.org/dc/terms/`. By searching for this URL
on the Wayback Machine at archive.org/web/, we can browse through
the archived snapshots of the page. Upon inspection, we can find
that the Dublin Core Terms namespace should be redirected to
`https://www.dublincore.org/specifications/dublin-core/dcmi-terms/`.

To find the redirect location for other URLs:

1. Visit archive.org/web/, which is the Wayback Machine's homepage.
2. In the search bar, enter the URL you want to check, such as `http://purl.org/dc/terms/`.
3. Once you search, you'll see a timeline of snapshots taken by the Wayback Machine. Click on a date to view the archived version of the page.
4. Look through the archived page to find any redirection information or links that indicate where the URL should point. In case of `dc/terms`
   it `https://www.dublincore.org/specifications/dublin-core/dcmi-terms/`.




## Contributing

If you have a redirect that you think should be added to the service, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Special thanks to Monica Chen for helping to get this out quickly
