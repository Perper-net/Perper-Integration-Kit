
# Perper.net ERP Integration Guide

This guide provides detailed instructions for integrating Perper.net's stablecoin payment system into your Enterprise Resource Planning (ERP) system. Follow these steps to set up and manage transactions effectively.

## Getting Started

To begin the integration process, you need to provide the following information to create a project on Perper.net - https://sdk.perper.net:

1. **Project Name**: The name of your project.
2. **Redirect URL**: The URL where users will be redirected after a transaction.
3. **API Endpoint**: The URL of your backend to which transaction results will be sent.

Upon project creation, you will receive:

- **Project ID**
- **Project Secret**

These credentials are crucial for the integration process and transaction verification.

## ERP Integration Requirements

For successful integration with your ERP system, you will need:

- **API Endpoint**: Your backend URL.
- **Project ID**: Provided by Perper.net.
- **Project Secret**: Provided by Perper.net.

## Transaction Handling

### Receiving Transaction Results

Your API Endpoint will receive transaction results in the following format:

- **For a failed transaction**:
  ```json
  {
    "status": "fail",
    "data": "string",
    "hash": "string"
  }
  ```

- **For a successful transaction**:
  ```json
  {
    "status": "success",
    "data": "string",
    "hash": "string",
    "transactionId": "string"
  }
  ```
  - `transactionId` is our internal ID for the completed transaction.

### Creating a QR Code for Perper Payments

To create a QR code for Perper payments, use a JSON string in this format:

```json
{
  "amount": number,
  "projectId": "string",
  "data": "string",
  "customId": "string"
}
```

- `amount`: The amount of Perper to be sent.
- `projectId`: Your Project ID.
- `data`: A string (can be randomly generated or an internal transaction ID) used later for verification.
- `customId` (optional): For ability to check the status of the transaction via the API. **NEEDS** to be unique

### Verification Process

The `data` field will be sent back to your API Endpoint, along with a signed JWT containing the `hash` field, using your Project Secret. Use these fields (`data` and `hash`) to validate whether the request is genuinely from us or a fraudulent attempt.

## Security and Validation

- Ensure the security of your Project ID and Project Secret.
- Validate each transaction by comparing the `data` and `hash` fields with your Project Secret.

## Checking if a transaction was successful with `customId`

If you've added the `customId` field when creating the QR Code, you can use it to see if the transaction was successful. You do that by sending a POST request to `https://backend.perper.net/sdk/transaction/checkStatus` with this kind of JSON body:

```json
{
  "projectId": "YOUR_PROJECT_ID",
  "customId": "UniqueTransactionId123",
  "hash": "HASHED_VALUE"
}
```

The `hash` field needs to be a `jwt` token signed with the project `secret`. The  payload must be JSON containing the `projectId` and `customId` fields, and they need to be the same as the ones that will be sent to the endpoint above.


### Response

If the request was successful the response will look like this:

```json
{
  "type": "success",
  "data": {
    // Whether the transaction was successful or not
    "successful": true | false
  }
}
```

And if the request wasn't successful the response will look like this:
```json
{
  "type": "error",
  "error": {
    "type": "SomeErrorType"
  }
}
```

## Support

For assistance or more information, contact [support@perper.net].
