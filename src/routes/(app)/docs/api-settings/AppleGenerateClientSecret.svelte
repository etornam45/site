<script>
    import Accordion from "@/components/Accordion.svelte";
    import CodeBlock from "@/components/CodeBlock.svelte";
    import SdkTabs from "@/components/SdkTabs.svelte";

    const responses = [
        {
            code: 200,
            body: `
                {
                    "secret": "..."
                }
            `,
        },
        {
            code: 400,
            body: `
                {
                  "code": 400,
                  "message": "Failed to generate client secret. Raw error:...",
                  "data": {}
                }
            `,
        },
        {
            code: 401,
            body: `
                {
                  "code": 401,
                  "message": "The request requires admin authorization token to be set.",
                  "data": {}
                }
            `,
        },
    ];

    let responseTab = responses[0].code;
</script>

<Accordion single>
    <svelte:fragment slot="header">
        <strong>Generate Apple client secret</strong>
    </svelte:fragment>

    <div class="content m-b-sm">
        <p>Generates a new Apple OAuth2 client secret key.</p>
    </div>

    <SdkTabs
        js={`
            import PocketBase from 'pocketbase';

            const pb = new PocketBase('http://127.0.0.1:8090');

            ...

            await pb.admins.authWithPassword('test@example.com', '1234567890');

            await pb.settings.generateAppleClientSecret(clientId, teamId, keyId, privateKey, duration)
        `}
        dart={`
            import 'package:pocketbase/pocketbase.dart';

            final pb = PocketBase('http://127.0.0.1:8090');

            ...

            await pb.admins.authWithPassword('test@example.com', '1234567890');

            await pb.settings.generateAppleClientSecret(clientId, teamId, keyId, privateKey, duration)
        `}
    />

    <div class="api-route alert alert-success">
        <strong class="label label-primary">POST</strong>
        <div class="content">/api/settings/apple/generate-client-secret</div>
        <small class="txt-hint auth-header">Requires <code>Authorization: TOKEN</code></small>
    </div>

    <div class="section-title">Body Parameters</div>
    <table class="table-compact table-border">
        <thead>
            <tr>
                <th>Param</th>
                <th>Type</th>
                <th width="50%">Description</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">clientId</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>The identifier of your app (aka. Service ID).</td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">teamId</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>
                    10-character string associated with your developer account (usually could be found next to
                    your name in the Apple Developer site).
                </td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">keyId</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>
                    10-character key identifier generated for the "Sign in with Apple" private key associated
                    with your developer account.
                </td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">privateKey</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>PrivateKey is the private key associated to your app.</td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">duration</span>
                    </div>
                </td>
                <td>
                    <span class="label">Number</span>
                </td>
                <td>
                    Duration specifies how long the generated JWT token should be considered valid.
                    <br />
                    The specified value must be in seconds and max 15777000 (~6months).
                </td>
            </tr>
        </tbody>
    </table>
    <small class="block txt-hint m-t-10 m-b-base">
        Body parameters could be sent as <em>JSON</em> or
        <em>multipart/form-data</em>.
    </small>

    <div class="section-title">Responses</div>
    <div class="tabs">
        <div class="tabs-header compact left">
            {#each responses as response (response.code)}
                <button
                    class="tab-item"
                    class:active={responseTab === response.code}
                    on:click={() => (responseTab = response.code)}
                >
                    {response.code}
                </button>
            {/each}
        </div>
        <div class="tabs-content">
            {#each responses as response (response.code)}
                <div class="tab-item" class:active={responseTab === response.code}>
                    <CodeBlock content={response.body} />
                </div>
            {/each}
        </div>
    </div>
</Accordion>
