# Cannot null subchart values repro

There's a bug that keeps resurfacing in Helm about nulling subchart value. Check https://github.com/helm/helm/pull/12879 for more info (in particular https://github.com/helm/helm/pull/12879#issuecomment-2163708473 to understand how painfully immortal this bug seems to be).

This repo contains two charts, upperchart and subchart (a dependency of upperchart). Only subchart renders something, with only a single value `theValue`.

Reproduction steps:
```bash
git clone https://github.com/pisto/helm-subchart-null-bug.git
cd helm-subchart-null-bug
helm dependency update --skip-refresh upperchart

# subchart just renders `theValueIs: theDefaultValue`, where the value is controlled by `.Values.theValue`
helm template subchart

# you can null out `theValue` when rendering the subchart
helm template subchart -f subchart-nulling-values.yaml

# you can override `theValue` when rendering the upperchart
helm template upperchart -f upperchart-override-values.yaml

# you CANNOT null out `theValue` when rendering the upperchart
helm template upperchart -f upperchart-nulling-values.yaml
```