## Background Functionality

### Script Commands

    $ ./bin/check-file-feed-age -h

    Usage: ./bin/check-file-feed-age [options]

    Options:
        -y, --yaml <path>                Specify configuration file <path> for runtime settings
        -l, --label <name>               Specify verbose product label.
        -p, --path-strftime <path>       The file path, may include 'strftime'-stamps.
        -w, --warning <time>             Specify the file-age warning boundary as <DD:HH:MM:SS>
        -c, --critical <time>            Specify the file-age critical boundary as <DD:HH:MM:SS>
        -v, --verbose [<integer>]        Specify output verbosity as <integer>, 0-3
        -m, --many-files [<boolean>]     Toggle many-files check algorithm via 'true' or 'false'
        -h, --help                       List all available options

### Configuring Time Boundaries

#### Vanilla Usage

    $ ./bin/check-file-feed-age -w 4000 -c 8000 -p /some/test/file/data.mkz

#### Advanced Usage

time definable in DAYS:HOURS:MINUTES:SECONDS

    $ ./bin/check-file-feed-age -w 5:00:00 -c -w 8:00:00 -p /some/test/file/data.mkz

flags are optional, default values exist

    $ ./bin/check-file-feed-age -c 1:05:30:6 -p /some/test/file/data.mkz

### Levels of Detail

boring...

    $ ./bin/check-file-feed-age -p /usr/local/catalog/catalog-nagios/config.ru
    Status: CRITICAL.

lets see some stats!

    $ ./bin/check-file-feed-age -p /usr/local/catalog/catalog-nagios/config.ru -v 3 -l "Look I Made Something"
    Status: CRITICAL.

    Feed Age: 4 days, 06:05:58

    File Location: /usr/local/catalog/catalog-nagios/config.ru

    Product label: Look I Made Something

    Additional Comparison Information
      Over warning bound by:         4 days, 05:55:58
      Over critical bound by:        4 days, 05:50:58

    Settings
      Warning Bound:                 00:10:00
      Critical Bound:                00:15:00
      Verbosity:                     3

### YAML Usage

configurations can get pretty complex...

    $ ./bin/check-file-feed-age -y config/products/some-yaml-config.yml

### "Many Files"

what is that "many files" command?

lets look at a real product for this.

    $ ./bin/check-file-feed-age -p /net/%DATESTAMP/research.Al_LMA.%DATETIMESTAMP.10minute_36kft.png -v 2
    Status: CRITICAL.

    Feed Age: 31 days, 08:16:18

    File Location: /net/20120703/research.Al_LMA.201207031212.10minute_36kft.png

that took a long time! That's a problem. Nagios times out its checks at around 10 seconds.

lets try the "many flags" feature.

    $ ./bin/check-file-feed-age -p /net/%DATESTAMP/research.Al_LMA.%DATETIMESTAMP.10minute_12kft.png -v 2 -m true -c 45:00:00:00
    Status: WARNING.

    Feed Age: 31 days, 08:15:08

    File Location: /net/20120703/research.Al_LMA.201207031212.10minute_12kft.png

whoah nice, that's quick! much quicker in fact, depending on the age of the file and the format of included timestamps, it can be several times faster.

"many files" has its downsides though, which is why we prefer to avoid "many files" on a default basis.

## Rake Tasks

another large component of this program is its ability to easily generate and move configuration files into place.

### Loading Data / Zith9 DB Interation

    $ rake -T nagios:data:
    rake nagios:data:load[file]  # Load Nagios checks from YAML fixtures file.
    rake nagios:data:unload[file]  # Unload Nagios checks from YAML fixtures file.

	rake nagios:config:deploy    # Deploy files to the deployment directory.
	rake nagios:config:forward   # Move forward one deployment revison.
	rake nagios:config:generate  # Generate Nagios config files from zith9 database.
	rake nagios:config:revert    # Revert back one deployment revision.