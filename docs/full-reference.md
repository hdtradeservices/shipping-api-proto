# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [shipping_api/stores/service.proto](#shipping_api_stores_service-proto)
    - [CreateRequest](#listing_api-CreateRequest)
    - [CreateResponse](#listing_api-CreateResponse)
    - [DeleteRequest](#listing_api-DeleteRequest)
    - [DeleteResponse](#listing_api-DeleteResponse)
    - [ListRequest](#listing_api-ListRequest)
    - [ListResponse](#listing_api-ListResponse)
    - [Store](#listing_api-Store)
    - [UpdateRequest](#listing_api-UpdateRequest)
    - [UpdateResponse](#listing_api-UpdateResponse)
  
    - [StoresService](#listing_api-StoresService)
  
- [Scalar Value Types](#scalar-value-types)



<a name="shipping_api_stores_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## shipping_api/stores/service.proto



<a name="listing_api-CreateRequest"></a>

### CreateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store | [Store](#listing_api-Store) |  |  |






<a name="listing_api-CreateResponse"></a>

### CreateResponse







<a name="listing_api-DeleteRequest"></a>

### DeleteRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store_id | [string](#string) |  |  |






<a name="listing_api-DeleteResponse"></a>

### DeleteResponse







<a name="listing_api-ListRequest"></a>

### ListRequest







<a name="listing_api-ListResponse"></a>

### ListResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| stores | [Store](#listing_api-Store) | repeated |  |






<a name="listing_api-Store"></a>

### Store
Store contains an ID and display name for a store


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| display_name | [string](#string) |  |  |






<a name="listing_api-UpdateRequest"></a>

### UpdateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store_id | [string](#string) |  |  |
| store | [Store](#listing_api-Store) |  |  |






<a name="listing_api-UpdateResponse"></a>

### UpdateResponse






 

 

 


<a name="listing_api-StoresService"></a>

### StoresService
StoresService provides a service for managing Stores
LATER: add links to guides around store definition and management

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| List | [ListRequest](#listing_api-ListRequest) | [ListResponse](#listing_api-ListResponse) | List retrieves a single listing by its ID |
| Create | [CreateRequest](#listing_api-CreateRequest) | [CreateResponse](#listing_api-CreateResponse) | Update creates a new store |
| Update | [UpdateRequest](#listing_api-UpdateRequest) | [UpdateResponse](#listing_api-UpdateResponse) | Update can be used to update existing stores |
| Delete | [DeleteRequest](#listing_api-DeleteRequest) | [DeleteResponse](#listing_api-DeleteResponse) | Delete can be used to delete existing stores |

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

