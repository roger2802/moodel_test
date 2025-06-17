Moodle Deployment with Ansible

This repository automates the installation and configuration of a Moodle instance using Ansible. It installs PostgreSQL, Apache, PHP 8.2 with required extensions, Redis for caching and sessions, and pulls the Moodle code from GitHub.

Table of Contents
	•	Prerequisites
	•	Inventory Structure
	•	Configuration
	•	Roles Overview
	•	Tags
	•	Usage
	•	License

Prerequisites
	•	Control machine with Ansible 2.18+ and Python 3.11+ installed
	•	SSH access to target servers in group moodle_servers
	•	Sudo privileges on target servers

Inventory Structure

```text
moodel_test/
├── inventory/
│   └── hosts.ini               # defines [moodle_servers]
├── group_vars/
│   └── moodle_servers.yml      # vars für postgres_root_password, db_name, etc.
├── roles/
│   ├── postgres/               # PostgreSQL installation & configuration
│   ├── apache/                 # Apache, PHP, Redis setup
│   └── moodle/                 # Moodle code checkout & config
├── site.yml                    # main playbook
├── requirements.yml            # Ansible Galaxy collections
└── README.md
```

Configuration
	•	group_vars/moodle_servers.yml: contains variables such as:

postgres_root_password: "yourPostgresRootPass"
db_name: moodledb
db_user: moodleuser
db_password: "yourDBPass"
moodle_version: "5.0.1"
moodle_host: "moodle.example.org"
moodle_www_root: "/var/www/moodle"
moodle_data_dir: "/var/moodledata"


	•	roles/apache/templates/vhost.conf.j2 and roles/moodle/templates/config.php.j2 are parameterized using these vars.

Roles Overview
	•	postgres: Installs PostgreSQL 14 from PGDG, sets up postgres password, creates Moodle database and user.
	•	apache: Installs Apache2, PHP 8.2 and extensions, configures max_input_vars, installs Redis and PHP-Redis, sets up VirtualHost.
	•	moodle: Clones the stable Moodle branch, sets configuration (config.php), enforces German UI, configures Redis cache and sessions.

Tags

You can limit execution by tags:
	•	--tags postgres  → Only PostgreSQL role
	•	--tags apache    → Only Apache/PHP/Redis role
	•	--tags moodle    → Only Moodle role

Usage
	1.	Install required collections:

ansible-galaxy collection install -r requirements.yml


	2.	Execute playbook:

ansible-playbook -i inventory/hosts.ini site.yml -K --tags "postgres,apache,moodle"


	3.	Access Moodle at http://moodle.example.org/ and follow the installer.

License

MIT ©