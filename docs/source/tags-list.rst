.. code-block:: bash
   :emphasize-lines: 1

   shell> ansible-playbook playbook.yml --list-tags
   
   playbook: playbook.yml

   play #1 (srv.example.com): srv.example.com TAGS: []
      TASK TAGS: [always, wpacli_action_script, wpacli_assert, wpacli_debug,
      wpacli_packages, wpacli_pid_dir, wpacli_rc, wpacli_rc_conf,
      wpacli_rc_defaults_patch, wpacli_rc_networksubr_patch, wpacli_rc_script,
      wpacli_wpa_cli]
