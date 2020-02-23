# Ansible Role: Transmission RSS

Installs [transmission-rss](https://github.com/nning/transmission-rss) for
Debian/Ubuntu linux machines. Also installs a init script under /etc/init.d/.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values:

    transmission_rss_version: 0.1.26

The transmission-rss gem package version to install.

    transmission_rss_service_name: transmission-rss

The service name that will be used for the init script, .pid and log files.
This allows multiple instances of the transmission-rss daemon to be installed
on the same machine.

    transmission_rss_config_path: /etc/transmission-rss.conf

The configuration file path.

    transmission_rss_config:
      feeds:
        - url: http://showrss.info/user/0.rss?magnets=true&namespaces=true&name=clean&quality=null&re=null
          download_path: /home/transmission-rss/Downloads
      update_interval: 600
      add_paused: false
      server:
        host: localhost
        port: 9091
        rpc_path: /transmission/rpc
      login:
        username: transmission
        password: transmission
      log_target: '/var/log/{{ transmission_rss_service_name }}.log'
      privileges:
        user: transmission-rss
        group: transmission-rss
      fork: true
      pid_file: false

The content of the transmission-rss configuration. For configuration examples
refer to
[the transmission-rss GitHub page](https://github.com/nning/transmission-rss).

## Dependencies

None.

## Example Playbook (using default config)

    - hosts: media-server
      roles:
        - tkanemoto.transmission-rss

## Example Playbook (custom configuration)

    - hosts: media-server
      roles:
        - tkanemoto.transmission-rss
          transmission_rss_config:
            feeds:
              - url: http://showrss.info/user/0.rss?magnets=true&namespaces=true&name=clean&quality=null&re=null
                download_path: /home/transmission-rss/Downloads
                regexp:
                  - matcher: '[Ss]ilicon[. ][Vv]alley'
                    download_path: '/mnt/share/media/TV/Silicon Valley'
                  - matcher: 'BBC'
                    download_path: '/mnt/share/media/TV/BBC'
            update_interval: 3600
            add_paused: false
            server:
              host: localhost
              port: 9091
              rpc_path: /transmission/rpc
            login:
              username: transmission
              password: transmission
            log_target: '/var/log/transmission-rss.log'
            privileges:
              user: transmission-rss
              group: transmission-rss
            fork: true
            pid_file: false

## License

MIT / BSD

## Author Information

This role was created in 2016 by [Takeshi Kanemoto](https://tkanemoto.com/).
