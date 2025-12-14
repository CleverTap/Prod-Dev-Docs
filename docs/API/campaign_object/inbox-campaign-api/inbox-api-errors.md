---
title: Inbox API Errors
excerpt: >-
  Learn about Inbox Channel API errors you may encounter if the API requests
  fail.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

If your Inbox channel API request fails, you will receive an error response, including a response code and a message explaining the reason for the error.

# Error Codes

The following table lists the errors you may encounter if there is an error in the Inbox channel API request:

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Error Code
      </th>

      <th style={{ textAlign: "left" }}>
        Error Description
      </th>

      <th style={{ textAlign: "left" }}>
        Details
      </th>

      <th style={{ textAlign: "left" }}>
        Status Code
      </th>

      <th style={{ textAlign: "left" }}>
        Response Payload
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        401
      </td>

      <td style={{ textAlign: "left" }}>
        Authentication Failed
      </td>

      <td style={{ textAlign: "left" }}>
        The AccountId and passcode provided in the headers are not authorized.
      </td>

      <td style={{ textAlign: "left" }}>
        401
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "fail",\
            "error": "Invalid Credentials",\
            "code": 401\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10001
      </td>

      <td style={{ textAlign: "left" }}>
        User not present in CleverTap database
      </td>

      <td style={{ textAlign: "left" }}>
        The user is not present in the CleverTap database.
      </td>

      <td style={{ textAlign: "left" }}>
        200
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "fail",\
            "error": "User not present",\
            "code": 10001\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10002
      </td>

      <td style={{ textAlign: "left" }}>
        Internal Error
      </td>

      <td style={{ textAlign: "left" }}>
        This can be caused due to internal exceptions.
      </td>

      <td style={{ textAlign: "left" }}>
        500
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "fail",\
            "error": "Internal Error",\
            "code": 10002\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10003
      </td>

      <td style={{ textAlign: "left" }}>
        Exceeded max messages/response size
      </td>

      <td style={{ textAlign: "left" }}>
        If the response size exceeds the specified limit, some messages are removed from the response until the total size is under the limit.
      </td>

      <td style={{ textAlign: "left" }}>
        200
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "success",\
            "error": "Exceeded max messages/response size",\
            "code": 10003\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10004
      </td>

      <td style={{ textAlign: "left" }}>
        Incorrect Payload format
      </td>

      <td style={{ textAlign: "left" }}>
        The payload received is incorrect. 
      </td>

      <td style={{ textAlign: "left" }}>
        400
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "fail",\
            "error": "Incorrect Payload Format",\
            "code": 10004\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10005
      </td>

      <td style={{ textAlign: "left" }}>
        Internal Server Error
      </td>

      <td style={{ textAlign: "left" }}>
        The CleverTap Inbox Server is down.
      </td>

      <td style={{ textAlign: "left" }}>
        503
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
            "status": "fail",\
            "error": "Internal Server Error",\
            "code": 10005\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10006
      </td>

      <td style={{ textAlign: "left" }}>
        Inbox API endpoints not enabled for account
      </td>

      <td style={{ textAlign: "left" }}>
        Endpoints are not enabled for the account.
      </td>

      <td style={{ textAlign: "left" }}>
        403
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
          "status": "fail",\
          "error": "Inbox API endpoints not enabled for account",\
          "code": 10006\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10007
      </td>

      <td style={{ textAlign: "left" }}>
        Exceeded max messages in a single request.
      </td>

      <td style={{ textAlign: "left" }}>
        The number of messages included in a single request exceeds the specified limit.
      </td>

      <td style={{ textAlign: "left" }}>
        400
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
          "status": "fail",\
          "error": "Exceeded max messages in a single request.",\
          "code": 10007\
        }
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        10008
      </td>

      <td style={{ textAlign: "left" }}>
        Endpoint invalid. Please check your endpoint and try again.
      </td>

      <td style={{ textAlign: "left" }}>
        The endpoint specified does not exist or is invalid.
      </td>

      <td style={{ textAlign: "left" }}>
        404
      </td>

      <td style={{ textAlign: "left" }}>
        \{\
          "status": "fail",\
          "error": "Endpoint invalid. Please check your endpoint and try again.",\
          "code": 10008\
        }
      </td>
    </tr>
  </tbody>
</Table>
