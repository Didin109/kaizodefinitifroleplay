import React, { useState, useEffect, createContext, useContext } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithCustomToken, onAuthStateChanged, signInAnonymously } from 'firebase/auth';
import { getFirestore, collection, doc, getDoc, setDoc, onSnapshot, query, orderBy, addDoc, updateDoc, deleteDoc, getDocs } from 'firebase/firestore';

// Context for Firebase services and user data
const FirebaseContext = createContext(null);

// Firebase Provider component
const FirebaseProvider = ({ children }) => {
    const [app, setApp] = useState(null);
    const [db, setDb] = useState(null);
    const [auth, setAuth] = useState(null);
    const [userId, setUserId] = useState(null);
    const [loadingFirebase, setLoadingFirebase] = useState(true);

    useEffect(() => {
        try {
            // Initialize Firebase app
            // Gunakan konfigurasi Firebase Anda secara langsung di sini untuk deployment di luar Canvas
            // Jika Anda menjalankan di Canvas, variabel global __firebase_config akan digunakan
            const firebaseConfig = typeof __firebase_config !== 'undefined'
                ? JSON.parse(__firebase_config)
                : {
                    apiKey: "AIzaSyC4YNnmBQE2nkD0fCQ_LEI_WAwK51ItebY", // Ganti dengan apiKey Firebase Anda
                    authDomain: "server-f20df.firebaseapp.com", // Ganti dengan authDomain Firebase Anda
                    projectId: "server-f20df", // Ganti dengan projectId Firebase Anda
                    storageBucket: "server-f20df.firebasestorage.app", // Ganti dengan storageBucket Firebase Anda
                    messagingSenderId: "1042741872334", // Ganti dengan messagingSenderId Firebase Anda
                    appId: "1:1042741872334:web:6c698e164bd6758ed9329f", // Ganti dengan appId Firebase Anda
                    measurementId: "G-JFN5SET77S" // Ganti dengan measurementId Firebase Anda
                };

            const firebaseApp = initializeApp(firebaseConfig);
            setApp(firebaseApp);

            // Get Firestore and Auth instances
            const firestoreDb = getFirestore(firebaseApp);
            setDb(firestoreDb);
            const firebaseAuth = getAuth(firebaseApp);
            setAuth(firebaseAuth);

            // Sign in with custom token (for Canvas environment) or anonymously (for general deployment)
            const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

            if (initialAuthToken) {
                signInWithCustomToken(firebaseAuth, initialAuthToken)
                    .then(() => {
                        console.log("Signed in with custom token.");
                    })
                    .catch((error) => {
                        console.error("Error signing in with custom token:", error);
                        signInAnonymously(firebaseAuth)
                            .then(() => console.log("Signed in anonymously."))
                            .catch((anonError) => console.error("Error signing in anonymously:", anonError));
                    })
                    .finally(() => setLoadingFirebase(false));
            } else {
                signInAnonymously(firebaseAuth)
                    .then(() => console.log("Signed in anonymously."))
                    .catch((error) => console.error("Error signing in anonymously:", error))
                    .finally(() => setLoadingFirebase(false));
            }

            // Listen for auth state changes to set userId
            const unsubscribe = onAuthStateChanged(firebaseAuth, (user) => {
                if (user) {
                    setUserId(user.uid);
                } else {
                    // Use a random ID if not authenticated, useful for anonymous users to have a consistent ID
                    setUserId(crypto.randomUUID());
                }
                setLoadingFirebase(false);
            });

            return () => unsubscribe(); // Cleanup auth listener on component unmount
        } catch (error) {
            console.error("Failed to initialize Firebase:", error);
            setLoadingFirebase(false);
        }
    }, []); // Empty dependency array means this effect runs once on mount

    return (
        <FirebaseContext.Provider value={{ app, db, auth, userId, loadingFirebase }}>
            {children}
        </FirebaseContext.Provider>
    );
};

// Custom hook to use Firebase services from the context
const useFirebase = () => useContext(FirebaseContext);

// Main App Component
const App = () => {
    // Destructure Firebase services and user data from context
    const { db, auth, userId, loadingFirebase } = useFirebase();

    // State variables for navigation and data
    const [currentPage, setCurrentPage] = useState('landing'); // 'landing', 'profile', 'store', 'chat'
    const [weapons, setWeapons] = useState([]); // List of weapons from Firestore
    const [selectedWeapon, setSelectedWeapon] = useState(null); // Weapon selected for purchase/chat
    const [chatMessages, setChatMessages] = useState([]); // Messages for the active chat ticket
    const [newMessage, setNewMessage] = useState(''); // Input for new chat messages
    const [activeChatTicketId, setActiveChatTicketId] = useState(null); // ID of the current active chat ticket
    const [showModal, setShowModal] = useState(false); // Controls visibility of info modal
    const [modalContent, setModalContent] = useState(''); // Content for the info modal
    const [isAdmin, setIsAdmin] = useState(false); // Flag to check if current user is admin
    const [isGeneratingDescription, setIsGeneratingDescription] = useState(false); // Loading state for LLM generation

    // Replace with your actual admin UID.
    // You can get this from Firebase Console -> Authentication -> Users after logging in as admin.
    const ADMIN_UID = 'YOUR_ADMIN_UID_HERE'; // <<< GANTI DENGAN UID ADMIN ANDA YANG SEBENARNYA

    // Effect to determine if the current user is an admin
    useEffect(() => {
        if (userId && userId === ADMIN_UID) {
            setIsAdmin(true);
        } else {
            setIsAdmin(false);
        }
    }, [userId, ADMIN_UID]); // Re-run if userId or ADMIN_UID changes

    // Effect to fetch weapons data from Firestore
    useEffect(() => {
        // Ensure db is initialized and Firebase is not still loading
        if (db && !loadingFirebase) {
            // Get the app ID, using a default if not defined (for local testing outside Canvas)
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the weapons collection in Firestore
            const weaponsColRef = collection(db, `artifacts/${appId}/public/data/weapons`);
            const q = query(weaponsColRef); // Create a query to get all documents in the collection

            // Set up a real-time listener for weapon data
            const unsubscribe = onSnapshot(q, (snapshot) => {
                // Map document snapshots to weapon objects
                const weaponsData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                setWeapons(weaponsData); // Update the weapons state
            }, (error) => {
                console.error("Error fetching weapons:", error);
                showInfoModal("Gagal memuat data senjata. Silakan coba lagi.");
            });

            return () => unsubscribe(); // Clean up the listener when the component unmounts
        }
    }, [db, loadingFirebase]); // Re-run if db or loadingFirebase changes

    // Effect to fetch chat messages for the active ticket
    useEffect(() => {
        // Ensure db, activeChatTicketId, userId are available and Firebase is not loading
        if (db && activeChatTicketId && !loadingFirebase && userId) {
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the messages sub-collection within a specific transaction ticket
            const chatMessagesColRef = collection(db, `artifacts/${appId}/users/${userId}/transactions/${activeChatTicketId}/messages`);
            // Query to order messages by timestamp
            const q = query(chatMessagesColRef, orderBy('timestamp'));

            // Set up a real-time listener for chat messages
            const unsubscribe = onSnapshot(q, (snapshot) => {
                const messages = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                setChatMessages(messages); // Update the chat messages state
            }, (error) => {
                console.error("Error fetching chat messages:", error);
                showInfoModal("Gagal memuat pesan chat. Silakan coba lagi.");
            });

            return () => unsubscribe(); // Clean up the listener
        }
    }, [db, activeChatTicketId, loadingFirebase, userId]); // Re-run if these dependencies change

    // Function to show a custom info modal
    const showInfoModal = (message) => {
        setModalContent(message);
        setShowModal(true);
    };

    // Handler for buying a weapon (initiates a chat transaction)
    const handleBuyWeapon = async (weapon) => {
        if (!db || !userId) {
            showInfoModal("Firebase belum siap atau pengguna tidak terautentikasi.");
            return;
        }

        try {
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the user's transactions collection
            const userTransactionsColRef = collection(db, `artifacts/${appId}/users/${userId}/transactions`);

            // Check if there's an existing open ticket for this user and weapon
            const q = query(userTransactionsColRef);
            const existingTicketsSnapshot = await getDocs(q);
            let existingTicket = null;

            existingTicketsSnapshot.forEach(doc => {
                if (doc.data().weaponId === weapon.id && doc.data().status === 'open') {
                    existingTicket = { id: doc.id, ...doc.data() };
                }
            });

            if (existingTicket) {
                // If an open ticket exists, activate it and inform the user
                setActiveChatTicketId(existingTicket.id);
                showInfoModal(`Anda sudah memiliki tiket transaksi terbuka untuk ${weapon.name}. Melanjutkan chat yang sudah ada.`);
            } else {
                // If no open ticket, create a new one
                const newTicketRef = await addDoc(userTransactionsColRef, {
                    weaponId: weapon.id,
                    weaponName: weapon.name,
                    buyerId: userId,
                    status: 'open', // Status of the transaction: 'open', 'completed', 'cancelled'
                    createdAt: new Date().toISOString(), // Timestamp of creation
                });
                setActiveChatTicketId(newTicketRef.id);
                showInfoModal(`Tiket transaksi baru untuk ${weapon.name} telah dibuat. Silakan mulai chat.`);
            }

            setSelectedWeapon(weapon); // Set the selected weapon for display in chat
            setCurrentPage('chat'); // Navigate to the chat page
        } catch (error) {
            console.error("Error creating/accessing transaction ticket:", error);
            showInfoModal("Gagal memulai transaksi. Silakan coba lagi.");
        }
    };

    // Handler for sending a chat message
    const handleSendMessage = async () => {
        if (!db || !userId || !activeChatTicketId || newMessage.trim() === '') {
            showInfoModal("Pesan tidak boleh kosong atau chat belum siap.");
            return;
        }

        try {
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the messages sub-collection of the active ticket
            const messagesColRef = collection(db, `artifacts/${appId}/users/${userId}/transactions/${activeChatTicketId}/messages`);

            await addDoc(messagesColRef, {
                senderId: userId,
                // Simple sender name: 'Admin' if current user is admin, 'Pembeli' otherwise
                senderName: userId === ADMIN_UID ? 'Admin' : 'Pembeli',
                message: newMessage,
                timestamp: Date.now(), // Timestamp for ordering messages
            });
            setNewMessage(''); // Clear the input field after sending
        } catch (error) {
            console.error("Error sending message:", error);
            showInfoModal("Gagal mengirim pesan. Silakan coba lagi.");
        }
    };

    // Handler for completing a transaction (only for admin)
    const handleCompleteTransaction = async () => {
        if (!db || !userId || !activeChatTicketId || !isAdmin) {
            showInfoModal("Anda tidak memiliki izin untuk menyelesaikan transaksi ini.");
            return;
        }

        try {
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the specific transaction document
            const transactionDocRef = doc(db, `artifacts/${appId}/users/${userId}/transactions`, activeChatTicketId);

            // Update the status of the transaction to 'completed'
            await updateDoc(transactionDocRef, {
                status: 'completed',
                completedAt: new Date().toISOString(), // Timestamp of completion
            });

            showInfoModal("Transaksi berhasil diselesaikan!");
            setActiveChatTicketId(null); // Clear the active ticket
            setChatMessages([]); // Clear chat messages
            setCurrentPage('store'); // Navigate back to the store page
        } catch (error) {
            console.error("Error completing transaction:", error);
            showInfoModal("Gagal menyelesaikan transaksi. Silakan coba lagi.");
        }
    };

    // Handler for updating weapon stock (only for admin)
    const handleUpdateStock = async (weaponId, newStock) => {
        if (!db || !isAdmin) {
            showInfoModal("Anda tidak memiliki izin untuk mengubah stok.");
            return;
        }
        if (newStock < 0) {
            showInfoModal("Stok tidak bisa kurang dari 0.");
            return;
        }

        try {
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            // Reference to the specific weapon document
            const weaponDocRef = doc(db, `artifacts/${appId}/public/data/weapons`, weaponId);
            await updateDoc(weaponDocRef, { stock: newStock }); // Update the stock field
            showInfoModal("Stok berhasil diperbarui!");
        } catch (error) {
            console.error("Error updating stock:", error);
            showInfoModal("Gagal memperbarui stok. Silakan coba lagi.");
        }
    };

    // New function to generate weapon description using Gemini API (only for admin)
    const handleGenerateWeaponDescription = async (weaponId, weaponName) => {
        if (!db || !isAdmin) {
            showInfoModal("Anda tidak memiliki izin untuk menghasilkan deskripsi.");
            return;
        }

        setIsGeneratingDescription(true); // Set loading state
        showInfoModal("Menghasilkan deskripsi senjata... Mohon tunggu."); // Inform user

        try {
            // Construct the prompt for the LLM
            const prompt = `Buatkan deskripsi roleplay singkat dan menarik (maksimal 3 kalimat) untuk senjata SAMP Definitive Roleplay bernama ${weaponName}. Fokus pada kegunaan IC (in-character) dan nuansa roleplay.`;
            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });

            const payload = { contents: chatHistory };
            const apiKey = ""; // Canvas will provide this API key at runtime
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            // Make the API call to Gemini
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });

            const result = await response.json(); // Parse the JSON response

            // Check if the response contains valid generated text
            if (result.candidates && result.candidates.length > 0 &&
                result.candidates[0].content && result.candidates[0].content.parts &&
                result.candidates[0].content.parts.length > 0) {
                const generatedText = result.candidates[0].content.parts[0].text;

                // Update the weapon's description in Firestore with the generated text
                const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
                const weaponDocRef = doc(db, `artifacts/${appId}/public/data/weapons`, weaponId);
                await updateDoc(weaponDocRef, { description: generatedText });

                showInfoModal(`Deskripsi untuk ${weaponName} berhasil diperbarui!`); // Success message
            } else {
                showInfoModal("Gagal menghasilkan deskripsi. Respon LLM tidak valid."); // Error if response is malformed
            }
        } catch (error) {
            console.error("Error generating weapon description:", error);
            showInfoModal("Terjadi kesalahan saat menghasilkan deskripsi. Silakan coba lagi."); // Catch network/API errors
        } finally {
            setIsGeneratingDescription(false); // Reset loading state
        }
    };

    // Function to render the current page based on currentPage state
    const renderPage = () => {
        // Show loading screen while Firebase is initializing
        if (loadingFirebase) {
            return (
                <div className="flex items-center justify-center min-h-screen bg-gray-900 text-white">
                    <div className="text-xl">Memuat Firebase...</div>
                </div>
            );
        }

        // Render different pages based on currentPage state
        return (
            <>
                {/* Landing Page */}
                {currentPage === 'landing' && (
                    <div className="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-gray-900 to-gray-700 text-white p-4">
                        <h1 className="text-5xl font-extrabold mb-6 text-yellow-400 drop-shadow-lg">
                            Toko Senjata IC SAMP Definitive Roleplay
                        </h1>
                        <p className="text-xl text-gray-300 mb-8 text-center max-w-2xl">
                            Selamat datang di toko senjata pribadi saya untuk Definitive Roleplay. Di sini Anda bisa menemukan berbagai senjata IC untuk kebutuhan Anda.
                        </p>
                        <div className="space-x-4">
                            <button
                                onClick={() => setCurrentPage('store')}
                                className="bg-yellow-500 hover:bg-yellow-600 text-gray-900 font-bold py-3 px-8 rounded-full shadow-lg transform transition duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-yellow-400 focus:ring-opacity-75"
                            >
                                Lihat Toko
                            </button>
                            <button
                                onClick={() => setCurrentPage('profile')}
                                className="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-8 rounded-full shadow-lg transform transition duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-75"
                            >
                                Profil GTA Saya
                            </button>
                        </div>
                    </div>
                )}
        
