# ansible-mvp-testing
Ansible minimum viable product examples playbook 

ansible-playbook -i 127.0.0.1, i~/tmp/test.yml

The Minimum Viable Playbook (MVP) is the shortest, most useful Ansible playbook I have. Whenever I need to write some Ansible code and I'm not entirely sure I'm doing it right (which is often), I implement it first in the MVP so I can test it quickly. Here's my latest iteration:

- hosts: 127.0.0.1
  connection: local
  gather_facts: no
  vars:
    my_name: a_b_c
  tasks:
    - debug: msg={{ my_name|replace('_', '-') }}
I keep the MVP saved as ~/tmp/test.yml, from which it can be run with:

$ ansible-playbook -i 127.0.0.1, ~/tmp/test.yml
(The -i argument allows you to specify the inventory as a comma-separated string of hosts, but it looks a little weird with only one host. You can't leave out the comma, otherwise it gets interpreted as a filename.)

The MVP runs in about 0.2 seconds giving me quick feedback:

PLAY [127.0.0.1] **************************************************************

TASK: [debug ] ****************************************************************
ok: [127.0.0.1] => {
    "msg": "a-b-c"
}

PLAY RECAP ********************************************************************
127.0.0.1                  : ok=1    changed=0    unreachable=0    failed=0
Here I was testing a rather trivial use of the replace filter, but since I wasn't sure I remembered how it worked, it was faster to use the MVP than to test the full task I was writing, or to go to the documentation. Running the actual playbook would take minutes rather than seconds (even with --start-at- task, since it relied on other tasks), and whilst going to the documentation is normally fast (I use Dash for quick local look- ups), but it can be tricky when the answer might be split between the Ansible and Jinja 2 docs.

Two things make the MVP particularly fast:

When running tasks locally, Ansible always uses the local connection type, which means there is no SSH overhead.

I've disabled fact-gathering which means my tasks are executed immediately. This saves about a second per run, which isn't much but you do notice it.

Hope this helps you. If there's something I'm missing, please let me know!
