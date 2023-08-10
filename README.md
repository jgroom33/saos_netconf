# saos_netconf

## Config generation using Jinja

`ansible-playbook mock.yml`

it will generate ./rpc.xml

## Single device exection (no inventory)

1. modify IP and creds in ./ciena_netconf_lone.yml
2. ` ansible-playbook ciena_netconf_lone.yml`

Note: the xml contents in ./rpc.xml do not have a root xml parent. This is because the `ansible.netcommon.netconf_rpc` module will add the root xml parent automatically. <edit-config> </edit-config> is injected as the root xml parent for the rpc edit-config operation.

```yml
  tasks:
    - name: Netconf execution
      ansible.netcommon.netconf_rpc:
        rpc: edit-config
        xmlns: "urn:ietf:params:xml:ns:netconf:base:1.0"
        content: |
          <target>
            <running />
          </target>
          <default-operation>merge</default-operation>
          <config>
            <fps xmlns="urn:ciena:params:xml:ns:yang:ciena-pn:ciena-mef-fp">
              <fp xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="remove">
                <name>foo</name>
              </fp>
            </fps>
          </config>
      connection: netconf
```

## Monitoring

Run this in a device to monitor the netconf session

```bash
log monitor netconf-rpc all
```
