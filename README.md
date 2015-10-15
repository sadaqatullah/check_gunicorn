# This is a plugin that works between ElasticSearch and Nagios. This plugin takes status report from ElasticSearch and puts it into Nagios. This plugin is specifically for Gunicorn working with statsd. Statsd takes data from Gunicorn and posts it into ElasticSearch. Then this plugin takes required data from the elasticsearch and transforms it for nagios. It tells Nagios states of workers working in Gunicorn, request rate that Gunicorn is facing, request ruration that Gunicorn is taking.

The file gets installed at '/usr/local/bin'

Inputs types to Plugin:

Case Type(-t) Input

Average Workers "Avgworkers"

Maximum Workers "Maxworkers"

Minimum Workers "Minworkers"

Average Request Rate "Avgrequestrate"

Maximum Request Rate "Maxrequestrate"

Minimum Request Rate "Minrequestrate"

Average Request Duration "Avgrequestduration"

Maximum Request Duration "Maxrequestduration"

Minimum Request Duration "Minrequestduration"

Set Time Range (-st): For now, Time Range is between (-st):currentsystemtime; where user can set -st to any number of minutes user wants to keep track of. By default -st is set to 5 minutes.

Warning Range (-w): Warning Range can be set by user in terms of range. Syntax is 'a:b', a is lower bound of range, b is upper bound of range.

Critical Range (-c): Critical Range can be set by user in terms of range. Syntax is 'a:b', a is lower bound of range, b is upper bound of range.

Commands "

Workers

define command { command_name check_gunicorn_maxworkers command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ } define command { command_name check_gunicorn_minworkers command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ }

RequestRate

define command { command_name check_gunicorn_requestrate command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ } define command { command_name check_gunicorn_requestratemax command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ }

RequestDuration

define command { command_name check_gunicorn_requestduration command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ } define command { command_name check_gunicorn_requestdurationmax command_line /usr/local/bin/check_gunicorn.py -st $ARG1$ -t $ARG2$ -w $ARG3$ -c $ARG4$ } " You might have differet path for check_gunicorn.py according to your pip configurations

Services "

Gunicorn Maximum Workers Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Max Workers check_command check_gunicorn_maxworkers!50!Maxworkers!:15!:25 }

Gunicorn Minimum Workers Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Min Workers check_command check_gunicorn_minworkers!50!Minworkers!10:!7: }

Gunicorn Request Rate Average Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Request Rate check_command check_gunicorn_requestrate!50!Avgrequestrate!5:15!4:25 }

Gunicorn Request Rate Maximum Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Request Rate Max check_command check_gunicorn_requestratemax!50!Maxrequestrate!:15!:25 }

Gunicorn Request Duration Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Request Duration check_command check_gunicorn_requestduration!50!Avgrequestduration!5:15!3:25 }

Gunicorn Request Duration Maximum Check

define service{ use local-service ; Name of service template to use host_name localhost service_description Gunicorn Request Duration Max check_command check_gunicorn_requestdurationmax!50!Maxrequestduration!:15!:25 }

"

NOTE: by default, application will show result of Maximum Workers in last 5 minutes.
