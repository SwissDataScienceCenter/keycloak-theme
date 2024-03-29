# keycloak-theme

This project defines the keycloak theme shown on RenkuLab.

## Production

In production, the Docker image is mounted into container running Keycloak. See the keycloak section of the helm chart/values file: https://github.com/SwissDataScienceCenter/renku/blob/master/helm-chart/renku/values.yaml. Search for `theme` to find the relevant lines.

## Configuration

Keycloak can be configured to prompt a user to accept the **terms of service** on their first login. In Keycloack version 20, this can be done in the admin UI under the `Authentication` section for the realm.

Select the `Required Actions` tab, make sure `Terms and Conditions` is enabled and set it as default action as well.

For a Keycloak version different from 20, consult the admin guide, since the configuration may be different.

## Development

For development, there is a `Dockerfile.dev` that can be built and run locally to shorten the feedback loop.

For development, you can do the following:

```
docker compose up -d
```

Then, after waiting for the service to start, connect to this URL

- http://localhost:8080/realms/renku/account/

From here, you can click _Sign In_ to get to the login UI.

You can make changes to the theme and refresh in your browser to see the updates. You need to do a hard refresh (e.g., Shift-Refresh on Safari) or ensure the cache is disabled to see your changes.

### Configuration

Keycloak can be configured to prompt a user to accept the **terms of service** on their first login. This is off by default, but to enable it, modify the following section in `renku-realm.json`:

```
{
    "alias": "terms_and_conditions",
    "name": "Terms and Conditions",
    "providerId": "terms_and_conditions",
    "enabled": false,
    "defaultAction": true,
    "priority": 20,
    "config": {}
}
```

and set `enabled` to `true`.

```
"enabled": true,
```

## Local Testing

After the initial theme is set up, you should make some configuration changes to test several scenarios:

### Auth providers

Configure some auth providers like

- Facebook http://localhost:8080/admin/master/console/#/renku/identity-providers/facebook/add
- GitHub http://localhost:8080/admin/master/console/#/renku/identity-providers/github/add

These should then appear on the login page.

### Reset password page

Enable the reset password page to make sure that looks correct:

http://localhost:8080/admin/master/console/#/renku/realm-settings/login

## Testing

If you want to deploy the theme in a real renku instance, you can build it and push it to docker hub, where you can reference it in a renku chart.

```
docker build -t [docker namespace]/renku-theme .
docker push [docker namespace]/renku-theme:latest
```

In the renku chart, change the `values.yaml` to reference the image in the `keycloak.extraInitContainers` section

## Structure

To understand the structure of the content, you may need to consult the source code for the theme.

Download the source for the release that is used (https://github.com/keycloak/keycloak/releases/tag/20.0.1) and then look in the `themes/src/main/resources/theme` folder for the `base` and `keycloak` templates. Official theme examples are found in `examples/themes/src/main/resources/theme` folder.
