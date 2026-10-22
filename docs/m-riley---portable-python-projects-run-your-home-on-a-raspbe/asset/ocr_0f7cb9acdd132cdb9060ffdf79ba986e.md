```
parser.add_argument('-t', action='store', dest='duration',
help='Number of minutes you wish to stream', required=True, type=int)

args = parser.parse_args()

duration = int(args.duration)*60

② try:
    cmd = "streamlink twitch.tv/" + args.channel + " 720p"
    twitchstream = subprocess.Popen(cmd,shell=True, stdout=subprocess.PIPE)

③ while True:
    line = str(twitchstream.stdout.readline())
    if not line:
        break
    if line.__contains__('error: Unable to validate '):
        print('Unable to connect to channel, either because \
channel is not broadcasting now or channel name is \
invalid. Quitting.')
        quit()
    if line.__contains__('Opening stream'):
        break

print('Stream ' + args.channel + ' for ' + str(duration) + ' seconds.')

④ time.sleep(duration)

⑤ if platform.system() == 'Windows':
    os.system("taskkill /IM vlc.exe")
else:
    os.killpg(os.getpgid(twitchstream.pid), signal.SIGTERM)

print('Streamed ' + args.channel + ' for ' + \
str(args.duration) + ' minutes.')

⑥ twitchstream.kill()
time.sleep(5)
os._exit(-1)
time.sleep(5)
sys.exit(-1)

finally:
    print('Stream closed.')
```