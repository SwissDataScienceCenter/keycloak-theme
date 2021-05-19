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

Then, after waiting for the service to start, connect to the url http://localhost:8080/auth/realms/renku/account/

You can make changes to the theme and refresh in your browser to see the updates. You need to do a hard refresh (e.g., Shift-Refresh on Safari) or ensure the cache is disabled to see your changes.

## Structure

To understand the structure of the content, you may need to consult the source code for the theme.

Download the source for the release that is used (https://github.com/keycloak/keycloak/releases/tag/11.0.3) and then look in the `themes/src/main/resources/theme` folder for the `base` and `keycloak` templates. Official theme examples are found in `examples/themes/src/main/resources/theme` folder.

