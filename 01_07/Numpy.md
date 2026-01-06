# ndarray
- N차원 배열 객체

# ndarray 생성 : array()
- 인자로 주로 파이썬 list 또는 ndarray 입력

# ndarray 생성 : arrange()
- 0부터 9까지의 숫자를 순차적으로 생성

# ndarray 생성 : zeros() / ones()
- (3,2) shape을 가지는 모든 원소가 0, dtype은 int32인 ndarray 생성

# 데이터 타입 : ndarray.dtype
- ndarray 내 데이터 타입은 같은 데이터 타입만 가능 -> ndarray.dtype 속성
- 즉, 한 개의 ndaaray 객체에 int와 float 함께 불가능 -> astype() 활용하여 형 변환

# 형태 : ndaaray.shape 속성
# 차원 : ndaaray.ndim 속성
## reshape() : 차원과 크기를 변경
- 차원을 변경할 때 요소의 개수가 맞지 않으면 ValueError 발생
## reshape(-1,N) : 차원과 크기를 변경
- -1에 해당하는 axis의 크기는 가변적
- -1이 아닌 인자 값에 해당하는 axis 크기는 인자 값으로 고정하여 shape 변환

