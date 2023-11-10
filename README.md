# akto-mitm-addon
This mitmproxy addon script can be used to populate the Akto inventory. 

`mitmproxy` An interactive TLS-capable intercepting HTTP proxy for penetration testers and software developers. More details can be found [here](https://mitmproxy.org/).

`Akto` is an open-source API security platform. More details can be found [here](https://www.akto.io/).

## How it works?
`mitmdump` is essentially the command-line version of mitmproxy, functioning much like tcpdump but for HTTP traffic. By utilizing this add-on script with the `-s` option, you can effectively save the API data collected in mitmdump to Akto. 

The script initializes a JSON object and starts populating it with HAR entries. When the JSON object reaches a size exceeding 20MB, it then proceeds to transmit the HAR JSON to Akto.

## Usage 
### Standalone mitmproxy
```bash
pip install mitmproxy requests
export AKTO_BASE_URL="http://192.168.10.10:9090"
export AKTO_API_KEY="abcdlkaskhjfskjlsadk"
mitmdump -s ./akto.py --set akto_collection=<Test123>

```
### Dockerized mitmproxy
```bash
docker build -t mitm .

docker run --rm -it \
  -v $(pwd):/opt/mitm \
  -p 8080:8080 \
  -e AKTO_BASE_URL="http://192.168.10.10:9090" \
  -e AKTO_API_KEY="abcdlkaskhjfskjlsadk" \
  mitm mitmdump -s /opt/mitm/akto.py \
  --set akto_collection=<Test123>
```

__Note__:  Upon completion of the execution, ensure the mitmdump is exited (or the mitmdump container is stopped) to transmit the remaining data to Akto. 

## Credits
This script is an adapted version of the [har_dump.py](https://github.com/mitmproxy/mitmproxy/blob/main/examples/contrib/har_dump.py) addon script, which has since been officially incorporated into mitmproxy.
