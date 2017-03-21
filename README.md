Green IoT Application Protocol
==============================

This repository contains the specification of an application level protocol which can be used to retrieve pre-calculated or raw
data from a data base into which sensor data is collected, possibly after some pre-processing.

The specification is currently in the form of a heavily documented C++ header file, with a programmatically enforced connection to
a corresponding JSON representation. This representation has a few rules worth mentioning:

- Each C++ object is represented by a JSON object. A specific JSON field called Type is used to describe the type of the C++
object, and has a text value equal to the name of the C++ class.

- Polymorphism is implemented but in this particular protocol it is used for the DataRequest::mRegion member and in the
DataReplyContainer::mReplies vector so in all other cases the Type field of C++ object representations is redundant.

- C++ member names have a m prefixed to their names. This m is removed in the JSON field name for object members.

- C++ std::vector objects are represented as JSON arrays.

- C++ enumerator values are prefixed with a c. This letter is (unfortunately) not removed when the enum value is converted to a
JSON string value. It is advices that decoders of the JSON data accept the enumerator values with or without the leading c character.


Running the protocol
====================

The protocol is designed to follow the REST principles. The top URL for a specific server must be known before the protocol can be
used. By performing a HTTP get on this URL the JSON representation of an InfoReply object is retrieved. This object contains
important information for applications to set up the user interface aqs well as the mDataURL which contains the URL to which
further DataRequests are to be directed.

DataRequest transactions are HTTP POST operations with the JSON representation of the DataRequest object as the post data. The
reply to the HTTP request contains the JSON encoded DataReplyContainer containing the entire response of the request.

