# SpringDataJPA 공통 필드 BaseEntity로 관리하기, Auditing으로 시간관리하기

여러 엔티티를 사용하면서 중복되는 필드(id, createdAt, updatedAt)를 반복적으로 생성해줘야 하는 불편함이 있다.

중복되는 필드를 BaseEntity를 활용하여 관리하는 방법을 알아보려고 한다.

## BaseEntity 생성

```java
@Getter
@MappedSuperclass
public class BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @CreationTimestamp
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @UpdateTimestamp
    @Column(name = "updated_at", nullable = false)
    private LocalDateTime updatedAt;
}
```

이렇게 중복되는 필드 id, createdAt, updatedAt을 작성해 주었다.

## @MappedSuperclass란?

@MappedSuperclass는 객체 상속을 통해 사용하지만 상속관계 매핑의 목적보다 **공통 매핑 정보가 필요**할 때 사용된다.

한마디로 위의 BaseEntity예시와 같이 중복되는 속성을 생성하여 Entity생성 시 상속받아 중복되는 속성을 처리하는 데에 의미를 둔다.

여기서 @MappedSuperclass가 **해당 BaseEntity를 엔티티로 인식되지 않게 하며, 데이터베이스에 테이블이 생성되지 않게 한다.**

### Entity 생성 예시

```java
// User Entity

@Getter
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User extends BaseEntity {

    @Column(nullable = false)
    private String email;

    private String password;

    @Column(nullable = false)
    private String name;

    private String nickname;

}
```

이렇게 BaseEntity를 사용함으로써 중복코드를 줄이 수 있으며 공통필드의 변경이 필요할 경우 부모 클래스에서만 수정하면 되므로 유지보스 측면에서도 장점이 있다.

## Auditing 이란?

Auditing의 사전적 의미는 감사, 심사이다.
Spring Data JPA에서는 Auditing 기능을 기본적으로 제공한다.

이를 사용하여 엔티티가 생성, 수정되는 시점을 감지하여 그 시간과 생성, 수정 한 사람을 기록하여 그 이력을 남길 수 있다.

## Configuration 작성

```java
// 2

@EnableJpaAuditing
@Configuration
public class AuditingConfig {
}
```

## BaseEntity에 적용하기

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class) // 추가
public class BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // 기존 어노테이션 @CreationTimestamp
    @CreatedDate // 변경된 어노테이션
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    // 기존 어노테이션 @UpdateTimestamp
    @LastModifiedDate // 변경된 어노테이션
    @Column(name = "updated_at", nullable = false)
    private LocalDateTime updatedAt;
}
```
