import json
import boto3

s3 = boto3.resource('s3')

def lambda_handler(event, context):
    
    bucket_list=[]
    for bucket in s3.buckets.all():
        print(bucket.name)
        bucket_list.append(bucket.name)
   
    subject= 'event from amuda'
    client = boto3.client('ses') 
    body = '''
           this is a notification mail regarding s3 event
           the bucket list are {} 
           '''.format(bucket_list)
    response = client.send_email(
    Source='amudaoluwaseun8@gmail.com',
    Destination={
        'ToAddresses': [
            'amudaoluwaseun8@gmail.com',
        ],
    },
    Message={
        'Subject': {
            'Data': subject,
        },
        'Body': {
            'Text': {
                'Data': body,
            },
           
        }
    },
)
    print('mail sent')