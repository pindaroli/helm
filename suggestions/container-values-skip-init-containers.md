# Container Values to Skip Init-Containers

Query: What are container per container .Values that skip init-container definition in templates

## **Sonarr** - Present without Init Container:
```yaml
sonarr:
  enabled: true
  persistence:
    enabled: false    # Skip init container but keep main container

# OR

jellyfin:
  enabled: false      # Skip init container but keep main container
sonarr:
  enabled: true

# OR  

jellyfin:
  persistence:
    media:
      enabled: false  # Skip init container but keep main container  
sonarr:
  enabled: true
```

## **Radarr** - Present without Init Container:
```yaml
radarr:
  enabled: true
  persistence:
    enabled: false    # Skip init container but keep main container

# OR

jellyfin:
  enabled: false      # Skip init container but keep main container
radarr:
  enabled: true

# OR

jellyfin:
  persistence:
    media:
      enabled: false  # Skip init container but keep main container
radarr:
  enabled: true
```

## **All Other Components** - Always Present without Auto Init Container:
```yaml
# These never have automatic init containers, only user-defined ones:
qbittorrent:
  enabled: true
  initContainers: []  # No init containers

lidarr:
  enabled: true  
  initContainers: []  # No init containers

readarr:
  enabled: true
  initContainers: []  # No init containers

bazarr:
  enabled: true
  initContainers: []  # No init containers

prowlarr:
  enabled: true
  initContainers: []  # No init containers

jellyseerr:
  enabled: true
  initContainers: []  # No init containers

flaresolverr:
  enabled: true
  initContainers: []  # No init containers

jellyfin:
  enabled: true
  # Never has init containers
```

## **Template Logic Reference:**

The init container creation logic is:
```yaml
{{- if and .Values.jellyfin.enabled .Values.jellyfin.persistence.media.enabled .Values.sonarr.persistence.enabled }}
initContainers:
  - name: init-directories
    # Creates /media/{component.persistence.path} directories
{{- end }}
```

**Key Point**: Only Sonarr and Radarr have automatic init container logic. All other components only use user-defined `initContainers: []` arrays.