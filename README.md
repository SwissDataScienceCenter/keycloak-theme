# keycloak-theme

This project defines the keycloak theme shown on RenkuLab.

# Production

In production, the Docker image is mounted into container running Keycloak. See the keycloak section of the helm chart/values file: https://github.com/SwissDataScienceCenter/renku/blob/master/helm-chart/renku/values.yaml. Search for `theme` to find the relevant lines.

# Development

For development, there is a `Dockerfile.dev` that can be built and run locally to shorten the feedback loop.

For development, you can do the following:

```
docker build -f Dockerfile.dev -t renku-theme .

docker run -d --rm --name renku-theme -v (pwd)/renku_theme:/opt/jboss/keycloak/themes/renku_theme -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin -e KEYCLOAK_IMPORT=/tmp/renku-realm.json renku-theme
```

Then, after waiting for the service to start, connect to one of these urls (depending of if you are testing the account or login page):
* http://localhost:8080/auth/realms/renku/account/
* http://localhost:8080/auth/realms/renku/protocol/openid-connect/auth?client_id=account&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fauth%2Frealms%2Frenku%2Faccount%2Flogin-redirect&state=1234&response_type=code&scope=openid

You can make changes to the theme and refresh in your browser to see the updates. You need to do a hard refresh (e.g., Shift-Refresh on Safari) or ensure the cache is disabled to see your changes.

## Testing

If you want to deploy the theme in a real renku instance, you can build it and push it to docker hub, where you can reference it in a renku chart.

```
docker build -t [docker namespace]/renku-theme .
docker push [docker namespace]/renku-theme:latest
```

In the renku chart, change the `values.yaml` to reference the image in the `keycloak.extraInitContainers` section

## Structure

To understand the structure of the content, you may need to consult the source code for the theme.

Download the source for the release that is used (https://github.com/keycloak/keycloak/releases/tag/15.0.2) and then look in the `themes/src/main/resources/theme` folder for the `base` and `keycloak` templates. Official theme examples are found in `examples/themes/src/main/resources/theme` folder.

