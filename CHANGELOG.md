## 1.0.0

Refactor Ansible playbooks and improve code readability

- Clean up the structure of site.yml and add descriptive play names
- Assign explicit tags to roles (postgres, apache, moodle)
- Extract variables from site.yml into dedicated vars files
- Reformat templates (config.php.j2, vhost.conf.j2) for better clarity
- Standardize PHP settings (max_input_vars, Redis sessions and cache)
- Optimize handlers and idempotent command tasks (phpenmod, git safe.directory)