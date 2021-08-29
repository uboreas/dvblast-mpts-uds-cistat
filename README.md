## dvblast-3.4 (mpts/uds/cistat patch)

dvblast creates MPTS output using `-d` switch but need to use a configuration file to filter SIDs or it's PIDs. In this case dvblast creates SPTS too, according to destinations specified in configuration file.

### MPTS output only (while using configuration file)
The main goal of this patch is to continue to use configuration file to get benefit from per stream options but preventing SPTS outputs and having just MPTS output using `-d` switch. This feature can be enabled using `-4 <config-file>` parameters.

Sample config file lines for `-4` switch (no SPTS destinations, just a placeholder);

	0       1       SID
	0       1       SID   PID,PID,PID

Usage example;

	> dvblast -4 file.conf ...


You can keep continue to use `-c <config-file>` parameters with the file which has original format of configuration lines when this feature is not necessary.

### Unix Domain Socket
The MPTS output can be sent to a UDS target. For this purpose the socket path will be defined with ***unix://*** prefix. In example, when the socket is created at "/var/run/sock", the `-d` swhich whill have parameter like;
 
	-d 'unix:///var/run/sock'

dvblast terminates if the socket file not exists or on an error while using the socket. 

### CI status exports
CI status information can be exported to a local file. The environment variable `DVBLAST_CISTATS` is used for this purpose since there is no switch left to use.

	> DVBLAST_CISTATS="/path/to/file.log" dvblast -4 file.conf -d unix:///path/to/sock ...

The log file will be updated periodically. The `-6` or `--print-period` switch can be used to set interval. The default value 2 seconds is used when not specified. 

Following information will be exported;

	CI exists
	CI type description
	Total number of CI slots
	Total number of descrambler slots
	Module present
	Module ready
	Module removed
	Module type
	Module code
	Module manufacturer code
	Module name
	Total number of supported CA IDs
	List of supported CA IDs
	Total number of PMTs attached
	MMI message
	Bitrate
	DVBlast version

### Patch
The patch file `dvblast-3.4-mpts.patch` is based on ***dvblast-3.4 (release)*** source code. A copy of the source code that the patch already implemented is located as `dvblast-3.4-mpts` in this project .

dvblast official page: 
http://www.videolan.org/projects/dvblast.html

