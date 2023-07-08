# Shortscan

An IIS short filename enumeration tool.

## Functionality

Shortscan is designed to quickly determine which files with short filenames exist on an IIS webserver. Once a short filename has been identified the tool will try to automatically identify the full filename.

In addition to standard discovery methods Shortscan also uses a unique checksum matching approach to attempt to find the long filename where the short filename is based on Windows' propriatary shortname collision avoidance checksum algorithm (more on this research at a later date).

## Installation

### Quick install

Using a recent version of [go](https://golang.org/):

```
go install github.com/bitquark/shortscan/cmd/shortscan@latest
```

### Manual install

To build (and optionally install) locally:

```
go get && go build
go install
```

## Usage

### Basic usage

Shortscan is easy to use with minimal configuration. Basic usage looks like:

```
$ shortscan http://example.org/
```

### Advanced features

The following options allow further tweaks:

```
$ shortscan --help
Shortscan v0.5 · an IIS short filename enumeration tool by bitquark
Usage: main [--wordlist FILE] [--header HEADER] [--concurrency CONCURRENCY] [--timeout SECONDS] [--verbosity VERBOSITY] [--fullurl] [--stabilise] [--patience LEVEL] [--characters CHARACTERS] [--autocomplete mode] [--isvuln] URL

Positional arguments:
  URL                    url to scan

Options:
  --wordlist FILE, -w FILE
                         combined wordlist + rainbow table generated with shortutil
  --header HEADER, -H HEADER
                         header to send with each request (use multiple times for multiple headers)
  --concurrency CONCURRENCY, -c CONCURRENCY
                         number of requests to make at once [default: 20]
  --timeout SECONDS, -t SECONDS
                         per-request timeout in seconds [default: 10]
  --verbosity VERBOSITY, -v VERBOSITY
                         how much noise to make (0 = quiet; 1 = debug; 2 = trace) [default: 0]
  --fullurl, -F          display the full URL for confirmed files rather than just the filename [default: false]
  --stabilise, -s        attempt to get coherent autocomplete results from an unstable server (generates more requests) [default: false]
  --patience LEVEL, -p LEVEL
                         patience level when determining vulnerability (0 = patient; 1 = very patient) [default: 0]
  --characters CHARACTERS, -C CHARACTERS
                         filename characters to enumerate [default: JFKGOTMYVHSPCANDXLRWEBQUIZ8549176320-_()&'!#$%@^{}~]
  --autocomplete mode, -a mode
                         autocomplete detection mode (auto = autoselect; method = HTTP method magic; status = HTTP status; distance = Levenshtein distance; none = disable) [default: auto]
  --isvuln, -V           bail after determining whether the service is vulnerable [default: false]
  --help, -h             display this help and exit
```

## Utility

Shortscan comes with a utility named `shortutil` for performing various short filename operations and to make custom rainbow tables for use with the tool.

For example, to create a rainbow table from an existing wordlist use:

```
shortutil wordlist input.txt > output.rainbow
```

### Full usage

```
Shortutil v0.2 // a short filename utility by bitquark
Usage: main <command> [<args>]

Options:
  --help, -h             display this help and exit

Commands:
  wordlist               add hashes to a wordlist for use with, for example, shortscan
  checksum               generate a one-off checksum for the given filename
```

## Credit

Original IIS short filename [research](https://soroush.secproject.com/downloadable/microsoft_iis_tilde_character_vulnerability_feature.pdf) by Soroush Dalili.

Additional research and this short filename scanner by [bitquark](https://github.com/bitquark).
