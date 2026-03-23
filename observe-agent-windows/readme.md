# Observe Agent: Windows Container
Windows Server Core `mcr.microsoft.com/windows/servercore:ltsc2025` was used as the base image.  

## Usage
Bring up the container using the following ocmmand:
```
docker run --name observe-agent -dp 4318:4318 -e OBSERVE_TOKEN="<tokenId>:<token>" -e OBSERVE_COLLECTION_ENDPOINT="https://<id>.collect.observeinc.com" observe-agent:windows-servercore
```
Data can be found in
- Logs > OpenTelemetery Logs (Host Explorer)
- Metrics > Host Explorer (Prometheus Metrics) > system_`<metric Name>`

## Notes
Windows Nano Server `FROM mcr.microsoft.com/windows/nanoserver:ltsc2025` does not work, error:
```
panic: Failed to find PdhOpenQuery procedure in pdh.dll: The specified procedure could not be found.

goroutine 1 [running]:
syscall.(*DLL).MustFindProc(...)
	/opt/hostedtoolcache/go/1.24.13/x64/src/syscall/dll_windows.go:127
github.com/open-telemetry/opentelemetry-collector-contrib/pkg/winperfcounters/internal/third_party/telegraf/win_perf_counters.init.1()
	/home/runner/work/observe-agent/observe-agent/vendor/github.com/open-telemetry/opentelemetry-collector-contrib/pkg/winperfcounters/internal/third_party/telegraf/win_perf_counters/pdh.go:199 +0x411
panic: Failed to find PdhOpenQuery procedure in pdh.dll: The specified procedure could not be found.

goroutine 1 [running]:
syscall.(*DLL).MustFindProc(...)
	/opt/hostedtoolcache/go/1.24.13/x64/src/syscall/dll_windows.go:127
github.com/open-telemetry/opentelemetry-collector-contrib/pkg/winperfcounters/internal/third_party/telegraf/win_perf_counters.init.1()
	/home/runner/work/observe-agent/observe-agent/vendor/github.com/open-telemetry/opentelemetry-collector-contrib/pkg/winperfcounters/internal/third_party/telegraf/win_perf_counters/pdh.go:199 +0x411
```
Disabling `host_monitoring` and `self_monitoring` also didn't work:
```
observe-agent.exe init-config --host_monitoring::enabled=false --self_monitoring::enabled=false
```

Size difference:
 - Windows Server Core - `4.84 GB`
 - Windows Nano Server - `0.48 GB`
