# install MIT krb5 (contains kadmin.local, kdb5_util, krb5.conf template)
brew install krb5

2. Create krb5.conf file with realm information
3. # create DB
mkdir data

kdb5_util create -s -r ARYAK.COM -d data/principal -sf data/.k5.ARYAK.COM

5 files will get created in data directory
Created the 6th file (.acl file) manually

# start kdc and kadmind (run in background / separate shells)
krb5kdc &
kadmind &

# create a user principal (alice) with password 'alicepwd'
kadmin.local -q "addprinc -pw user1pass user1@ARYAK.COM"

# create a service principal for the DB server; suppose the DB hostname you use is 'localhost:5435'
kadmin.local -q "addprinc -randkey pg/localhost:5435@ARYAK.COM"

# extract the service principal keys into a keytab file for the DB server
kadmin.local -q "ktadd -k pg.keytab pg/localhost:5435@ARYAK.COM"
