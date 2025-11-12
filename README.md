# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
```
import socket
def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request.encode())
        response = b""
        while True:
            data = s.recv(4096)
            if not data:
                break
            response += data
    return response.decode(errors='ignore')
def upload_file(host, port, filename):
    with open(filename, 'r') as file:
        file_data = file.read()
        content_length = len(file_data)
        request = (
            f"POST /post HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Type: text/plain\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Connection: close\r\n\r\n"
            f"{file_data}"
        )
        response = send_request(host, port, request)
    return response
def download_file(host, port, filename):
    request = (
        f"GET /get HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Connection: close\r\n\r\n"
    )
    response = send_request(host, port, request)
    with open(filename, 'w') as file:
        file.write(response)
    return response
if __name__ == "__main__":
    host = 'httpbin.org'  
    port = 80
    with open('example.txt', 'w') as f:
        f.write("This is a test upload via socket program.")

    print("Uploading file...")
    upload_response = upload_file(host, port, 'example.txt')
    print("\n=== Upload Response ===\n")
    print(upload_response[:500]) 
    print("\nDownloading data...")
    download_response = download_file(host, port, 'downloaded.txt')
    print("\n=== Download Response Saved to downloaded.txt ===")
```
## OUTPUT
<img width="719" height="740" alt="image" src="https://github.com/user-attachments/assets/bb57b15d-2e6a-49d4-bfae-bd982fbeca64" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
