const ExpensiveComponent = React.memo(({ data }) => {
  // Only re-renders if data changes
  return <div>{data.map(item => <Item key={item.id} {...item} />)}</div>;
});

const CallbackExample = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []); // Memoized callback

  return <Button onClick={handleClick}>Count: {count}</Button>;
};

// Lazy loading components
const LazyComponent = React.lazy(() => import('./LazyComponent'));

const VirtualList = ({ items }) => {
  const [startIndex, setStartIndex] = useState(0);
  const visibleItems = items.slice(startIndex, startIndex + 10);

  return (
    <div onScroll={handleScroll}>
      {visibleItems.map(item => (
        <ListItem key={item.id} {...item} />
      ))}
    </div>
  );
};

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <LazyComponent />
    </Suspense>
  );
}
