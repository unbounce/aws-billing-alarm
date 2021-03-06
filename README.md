# Billing Alarm stack

This project creates the minimum AWS resources necessary to alert when
a the monthly billing charges exceeds a given threshold. The resources
are managed with a CloudFormation stack.

One design decision made is to add a basic email address as the only
subscriber to the SNS topic receiving the Cloudwatch alarm.  This is
intended to be the account's email address (supplied by the caller).
If another process wants to subscribe to the SNS topic, create another
CloudFormation stack using the exported stack output for the SNS
topic ARN.

## Requirements

* Ansible 2.2 (used for orchestrating the stack)
* AWS access
* Permission to create/delete CloudFormation stacks, SNS topics, and
CloudWatch alarms.

Ansible is required if you want full orchestration of the stack, but if
you are comfortable launching the CloudFormation stack you can consider
the Ansible components as optional.  If you are new to this project, use
Ansible.

## Launching this Stack

Given some values for `EMAIL_ADDRESS`, `ACCOUNT_NAME`, and `MAX_DOLLARS`
you launch the stack with the following command:

```
$ ansible-playbook -i localhost.inventory -e 'recipient_email=EMAIL_ADDRESS account_name=ACCOUNT_NAME max_expense_in_dollars=MAX_DOLLARS' create-stack.yml

PLAY [all] *********************************************************************

TASK [load project settings] ***************************************************
ok: [localhost]

TASK [ensure cloudformation stack is created or deleted] ***********************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

Once Ansible is complete, the stack has been created.

Take note that the SNS topic subscription will need to be confirmed, which
arrives as an email to the email address's inbox specified in the call to
launch the stack.

## License

tl;dr MIT license

See [LICENSE](LICENSE) for more details.

