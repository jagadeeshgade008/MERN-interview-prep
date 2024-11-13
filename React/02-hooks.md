const [state, setState] = useState(initialState);

// Example
const NameInput = () => {
  const [name, setName] = useState('');
  
  return (
    <input 
      value={name}
      onChange={(e) => setName(e.target.value)}
    />
  );
};

// Basic Usage
useEffect(() => {
  // Effect code
  return () => {
    // Cleanup code
  };
}, [dependencies]);

// Example
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const fetchUser = async () => {
      const data = await api.getUser(userId);
      setUser(data);
    };
    fetchUser();
  }, [userId]);

  return <div>{user?.name}</div>;
};

const ThemeContext = React.createContext('light');

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

const InputFocus = () => {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
};
