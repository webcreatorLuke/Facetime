import React, { useState, useRef, useEffect } from 'react';
import { Video, VideoOff, Mic, MicOff, PhoneOff, Copy, Check } from 'lucide-react';

export default function NexusVideoChat() {
  const [stage, setStage] = useState('home');
  const [roomCode, setRoomCode] = useState('');
  const [inputCode, setInputCode] = useState('');
  const [myName, setMyName] = useState('');
  const [videoEnabled, setVideoEnabled] = useState(true);
  const [audioEnabled, setAudioEnabled] = useState(true);
  const [copied, setCopied] = useState(false);
  const [error, setError] = useState('');
  const [participantCount, setParticipantCount] = useState(1);

  const localVideoRef = useRef(null);
  const localStreamRef = useRef(null);
  const peerRef = useRef(null);
  const connectionsRef = useRef(new Map());
  const participantsRef = useRef(new Set());
  const [remoteStreams, setRemoteStreams] = useState([]);

  useEffect(() => {
    // Load PeerJS
    const script = document.createElement('script');
    script.src = 'https://unpkg.com/peerjs@1.5.1/dist/peerjs.min.js';
    script.async = true;
    document.head.appendChild(script);

    return () => {
      cleanup();
    };
  }, []);

  const generateName = () => {
    const adjectives = ['Red', 'Blue', 'Green', 'Purple', 'Orange', 'Yellow', 'Pink', 'Cyan'];
    const nouns = ['Fox', 'Wolf', 'Bear', 'Eagle', 'Tiger', 'Lion', 'Hawk', 'Shark'];
    return adjectives[Math.floor(Math.random() * adjectives.length)] + ' ' +
           nouns[Math.floor(Math.random() * nouns.length)];
  };

  const startLocalStream = async () => {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({
        video: true,
        audio: true
      });
      localStreamRef.current = stream;
      if (localVideoRef.current) {
        localVideoRef.current.srcObject = stream;
      }
      return stream;
    } catch (err) {
      setError('Failed to access camera/microphone. Please allow permissions.');
      throw err;
    }
  };

  const createRoom = async () => {
    try {
      setError('');
      await startLocalStream();
      
      const code = Math.floor(10000 + Math.random() * 90000).toString();
      const name = generateName();
      
      setRoomCode(code);
      setMyName(name);
      
      if (!window.Peer) {
        setError('Loading... Please wait a moment and try again.');
        return;
      }

      const peer = new window.Peer('room_' + code);
      peerRef.current = peer;

      peer.on('open', (id) => {
        console.log('Room created:', code);
        setStage('connected');
      });

      peer.on('connection', (conn) => {
        handleIncomingConnection(conn);
      });

      peer.on('call', (call) => {
        handleIncomingCall(call);
      });

      peer.on('error', (err) => {
        console.error('Peer error:', err);
        setError('Connection error. Please try again.');
      });

    } catch (err) {
      console.error('Error creating room:', err);
    }
  };

  const joinRoom = async () => {
    if (inputCode.length !== 5) {
      setError('Please enter a valid 5-digit code');
      return;
    }

    try {
      setError('');
      await startLocalStream();
      
      const code = inputCode;
      const name = generateName();
      
      setRoomCode(code);
      setMyName(name);

      if (!window.Peer) {
        setError('Loading... Please wait a moment and try again.');
        return;
      }

      const myPeerId = 'user_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
      const peer = new window.Peer(myPeerId);
      peerRef.current = peer;

      peer.on('open', (id) => {
        console.log('Joining room:', code);
        
        const hostId = 'room_' + code;
        const conn = peer.connect(hostId, { reliable: true });
        
        conn.on('open', () => {
          conn.send({ type: 'join', name: name, peerId: id });
          
          const call = peer.call(hostId, localStreamRef.current);
          if (call) {
            call.on('stream', (remoteStream) => {
              addRemoteStream(hostId, remoteStream);
            });
          }
          
          setStage('connected');
        });

        conn.on('error', (err) => {
          console.error('Connection error:', err);
          setError('Could not connect to room. Make sure the code is correct.');
        });
      });

      peer.on('connection', (conn) => {
        handleIncomingConnection(conn);
      });

      peer.on('call', (call) => {
        handleIncomingCall(call);
      });

      peer.on('error', (err) => {
        console.error('Peer error:', err);
        setError('Connection error. Room may not exist.');
      });

    } catch (err) {
      console.error('Error joining room:', err);
    }
  };

  const handleIncomingConnection = (conn) => {
    conn.on('open', () => {
      connectionsRef.current.set(conn.peer, conn);
    });

    conn.on('data', (data) => {
      if (data.type === 'join') {
        participantsRef.current.add(data.peerId);
        setParticipantCount(participantsRef.current.size + 1);
      }
    });
  };

  const handleIncomingCall = (call) => {
    call.answer(localStreamRef.current);
    
    call.on('stream', (remoteStream) => {
      addRemoteStream(call.peer, remoteStream);
      participantsRef.current.add(call.peer);
      setParticipantCount(participantsRef.current.size + 1);
    });
  };

  const addRemoteStream = (peerId, stream) => {
    setRemoteStreams(prev => {
      const filtered = prev.filter(s => s.peerId !== peerId);
      return [...filtered, { peerId, stream }];
    });
  };

  const endCall = () => {
    cleanup();
    setStage('home');
    setRoomCode('');
    setInputCode('');
    setError('');
    setRemoteStreams([]);
    setParticipantCount(1);
    participantsRef.current.clear();
  };

  const cleanup = () => {
    if (localStreamRef.current) {
      localStreamRef.current.getTracks().forEach(track => track.stop());
      localStreamRef.current = null;
    }
    
    connectionsRef.current.forEach(conn => {
      try { conn.close(); } catch (e) {}
    });
    connectionsRef.current.clear();

    if (peerRef.current) {
      try { peerRef.current.destroy(); } catch (e) {}
      peerRef.current = null;
    }
  };

  const toggleVideo = () => {
    if (localStreamRef.current) {
      const videoTrack = localStreamRef.current.getVideoTracks()[0];
      if (videoTrack) {
        videoTrack.enabled = !videoTrack.enabled;
        setVideoEnabled(videoTrack.enabled);
      }
    }
  };

  const toggleAudio = () => {
    if (localStreamRef.current) {
      const audioTrack = localStreamRef.current.getAudioTracks()[0];
      if (audioTrack) {
        audioTrack.enabled = !audioTrack.enabled;
        setAudioEnabled(audioTrack.enabled);
      }
    }
  };

  const copyCode = () => {
    navigator.clipboard.writeText(roomCode);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-950 via-blue-950 to-slate-900 flex items-center justify-center p-4 font-sans">
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Inter:wght@400;500;600&display=swap');
        
        .glow {
          box-shadow: 0 0 20px rgba(6, 182, 212, 0.5),
                      0 0 40px rgba(6, 182, 212, 0.3),
                      0 0 60px rgba(6, 182, 212, 0.1);
        }

        .text-glow {
          text-shadow: 0 0 10px rgba(6, 182, 212, 0.8),
                       0 0 20px rgba(6, 182, 212, 0.5),
                       0 0 30px rgba(6, 182, 212, 0.3);
        }

        video {
          transform: scaleX(-1);
        }

        @keyframes slideUp {
          from {
            opacity: 0;
            transform: translateY(20px);
          }
          to {
            opacity: 1;
            transform: translateY(0);
          }
        }

        .slide-up {
          animation: slideUp 0.4s ease-out;
        }
      `}</style>

      {stage === 'home' && (
        <div className="slide-up max-w-md w-full">
          <div className="text-center mb-8">
            <h1 className="text-6xl font-black mb-3 text-glow" style={{ fontFamily: 'Orbitron, sans-serif', color: '#06b6d4' }}>
              NEXUS
            </h1>
            <p className="text-cyan-300 text-sm tracking-widest mb-2" style={{ fontFamily: 'Inter, sans-serif' }}>
              PEER-TO-PEER VIDEO CHAT
            </p>
            <p className="text-cyan-400 text-xs" style={{ fontFamily: 'Inter, sans-serif' }}>
              Works across different devices
            </p>
          </div>

          <div className="space-y-4">
            <button
              onClick={createRoom}
              className="w-full bg-gradient-to-r from-cyan-500 to-blue-500 text-white py-4 px-6 rounded-lg font-semibold text-lg glow hover:scale-105 transition-transform duration-300"
              style={{ fontFamily: 'Inter, sans-serif' }}
            >
              CREATE ROOM
            </button>

            <div className="relative">
              <div className="absolute inset-0 flex items-center">
                <div className="w-full border-t border-cyan-800"></div>
              </div>
              <div className="relative flex justify-center text-sm">
                <span className="px-4 bg-slate-950 text-cyan-400" style={{ fontFamily: 'Inter, sans-serif' }}>
                  OR
                </span>
              </div>
            </div>

            <div className="space-y-3">
              <input
                type="text"
                maxLength="5"
                placeholder="ENTER 5-DIGIT CODE"
                value={inputCode}
                onChange={(e) => setInputCode(e.target.value.replace(/\D/g, ''))}
                className="w-full bg-slate-900/50 border-2 border-cyan-700 text-cyan-100 py-3 px-4 rounded-lg text-center text-2xl font-bold tracking-widest focus:outline-none focus:border-cyan-400 transition-colors"
                style={{ fontFamily: 'Orbitron, sans-serif' }}
              />
              <button
                onClick={joinRoom}
                className="w-full bg-slate-800 border-2 border-cyan-600 text-cyan-300 py-4 px-6 rounded-lg font-semibold text-lg hover:bg-slate-700 hover:border-cyan-400 transition-all duration-300"
                style={{ fontFamily: 'Inter, sans-serif' }}
              >
                JOIN ROOM
              </button>
            </div>

            {error && (
              <div className="bg-red-900/30 border border-red-500 text-red-300 px-4 py-3 rounded-lg text-sm text-center">
                {error}
              </div>
            )}
          </div>
        </div>
      )}

      {stage === 'connected' && (
        <div className="slide-up w-full h-screen flex flex-col">
          <div className="bg-slate-900/70 backdrop-blur-md border-b-2 border-cyan-700 p-4 flex justify-between items-center flex-wrap gap-4">
            <div className="flex items-center gap-4">
              <div className="flex items-center gap-2 bg-cyan-600/20 border-2 border-cyan-500 px-4 py-2 rounded-lg">
                <span className="text-2xl font-bold text-cyan-300 tracking-wider" style={{ fontFamily: 'Orbitron, sans-serif' }}>
                  {roomCode}
                </span>
                <button onClick={copyCode} className="p-1 hover:bg-cyan-600/30 rounded transition-colors">
                  {copied ? <Check className="w-5 h-5 text-cyan-300" /> : <Copy className="w-5 h-5 text-cyan-300" />}
                </button>
              </div>
              <span className="text-cyan-400 text-sm">
                <span className="font-bold text-cyan-300">{participantCount}</span> online
              </span>
            </div>

            <div className="flex gap-2">
              <button
                onClick={toggleVideo}
                className={`p-3 rounded-lg transition-all ${
                  videoEnabled
                    ? 'bg-slate-800 border-2 border-cyan-600 text-cyan-300'
                    : 'bg-red-600 border-2 border-red-500 text-white'
                }`}
              >
                {videoEnabled ? <Video className="w-5 h-5" /> : <VideoOff className="w-5 h-5" />}
              </button>

              <button
                onClick={toggleAudio}
                className={`p-3 rounded-lg transition-all ${
                  audioEnabled
                    ? 'bg-slate-800 border-2 border-cyan-600 text-cyan-300'
                    : 'bg-red-600 border-2 border-red-500 text-white'
                }`}
              >
                {audioEnabled ? <Mic className="w-5 h-5" /> : <MicOff className="w-5 h-5" />}
              </button>

              <button
                onClick={endCall}
                className="p-3 rounded-lg bg-red-600 border-2 border-red-500 text-white hover:bg-red-700 transition-colors"
              >
                <PhoneOff className="w-5 h-5" />
              </button>
            </div>
          </div>

          <div className="flex-1 p-4 overflow-y-auto">
            <div className="grid gap-4" style={{ gridTemplateColumns: 'repeat(auto-fill, minmax(300px, 1fr))' }}>
              {/* Local video */}
              <div className="relative aspect-video bg-slate-950 rounded-xl overflow-hidden border-2 border-cyan-700 glow">
                <video
                  ref={localVideoRef}
                  autoPlay
                  playsInline
                  muted
                  className="w-full h-full object-cover"
                />
                {!videoEnabled && (
                  <div className="absolute inset-0 bg-slate-900 flex items-center justify-center">
                    <div className="w-16 h-16 rounded-full bg-gradient-to-br from-cyan-500 to-blue-500 flex items-center justify-center text-white text-2xl font-bold" style={{ fontFamily: 'Orbitron' }}>
                      {myName.split(' ').map(n => n[0]).join('')}
                    </div>
                  </div>
                )}
                <div className="absolute bottom-4 left-4 bg-slate-900/80 backdrop-blur-sm px-3 py-1 rounded-full border border-cyan-600">
                  <span className="text-cyan-300 text-sm font-semibold" style={{ fontFamily: 'Inter, sans-serif' }}>
                    {myName} (You)
                  </span>
                </div>
              </div>

              {/* Remote videos */}
              {remoteStreams.map(({ peerId, stream }) => (
                <RemoteVideo key={peerId} stream={stream} />
              ))}
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

function RemoteVideo({ stream }) {
  const videoRef = useRef(null);

  useEffect(() => {
    if (videoRef.current && stream) {
      videoRef.current.srcObject = stream;
    }
  }, [stream]);

  return (
    <div className="relative aspect-video bg-slate-950 rounded-xl overflow-hidden border-2 border-cyan-800">
      <video
        ref={videoRef}
        autoPlay
        playsInline
        className="w-full h-full object-cover"
      />
      <div className="absolute bottom-4 left-4 bg-slate-900/80 backdrop-blur-sm px-3 py-1 rounded-full border border-cyan-600">
        <span className="text-cyan-300 text-sm font-semibold" style={{ fontFamily: 'Inter, sans-serif' }}>
          Participant
        </span>
      </div>
    </div>
  );
}
