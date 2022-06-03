# digall

You have a few different options for how you can run it.  The "script" version
(Option 1) is a little more complete.  It has a help message (`-h|--help`) and
supports a `-s|--show` argument that will print out the dig command that it's
using, which is useful for learning.

## Option 1:  Install as a script

You can download it as a script file with `wget` by running this command:

```bash
wget https://raw.githubusercontent.com/FreedomBen/digall/main/digall \
  && chmod +x digall
```

Now use it with whatever domains you like:

```bash
./digall example.com
./digall something.example.com
```

If you add it to [somewhere in your `$PATH` variable](https://opensource.com/article/17/6/set-path-linux) then you can invoke it without the `./` prefix:

```bash
# Install in /usr/local/bin
sudo curl -L https://raw.githubusercontent.com/FreedomBen/digall/main/digall -o "/usr/local/bin/digall"
sudo chmod +x /usr/local/bin/digall

# assumes that /usr/local/bin is in the $PATH
digall example.com
```

## Option 2:  As a bash function temporarily

You can use `digall` as a "bash function" that is loaded in the current shell.  To make available in your current shell, copy and paste this:

```bash
digall()
{
  local color_restore='\033[0m'
  local color_red='\033[0;31m'
  local color_light_green='\033[1;32m'
  local color_light_blue='\033[1;34m'
  local color_light_cyan='\033[1;36m'

  if [ -z "$1" ]; then
    echo -e "${color_red}Error: Please pass domain as first arg${color_restore}"
  else
    echo -e "${color_light_blue}Queries: (dig +noall +answer '$1' '<type>')...${color_light_cyan}\n"
    # CNAME isn't needed because it will show up as the other record types
    for t in SOA NS SPF TXT MX AAAA A; do
      echo -e "${color_light_green}Querying for $t records...${color_restore}${color_light_cyan}"
      dig +noall +answer "$1" "${t}"
      echo -e "${color_restore}"
    done
  fi
}
```

Now you can use it with any domain or subdomain.  Examples:

```
digall example.com
digall something.example.com
```

## Option 3:  As a bash function that is persistent (available in every shell)

Copy and paste the same snippet above that starts with `digall()` and paste it into your bashrc file, usually located at `~/.bashrc`.

New shells that you spawn will have the function available automatically.  Existing shells can be loaded with it by:

```bash
. ~/.bashrc
```

Now you can use it with any domain or subdomain.  Examples:

```
digall example.com
digall something.example.com
```
